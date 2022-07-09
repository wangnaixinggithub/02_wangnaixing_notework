# Use-POI-importExcel

# 1、前端的字节流 => MultipartFile

```xml
      <!--POI依赖-->
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
            <version>3.17</version>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>3.17</version>
        </dependency>
```

```java
    @SneakyThrows
    @ApiOperation("从Excel导入会员列表")
    @RequestMapping(value = "/importList", method = RequestMethod.POST)
    @ResponseBody
    public CommonResult importList(@RequestPart("file") MultipartFile file) {
        
    	 Workbook workbook = ExcelHelper.getWorkbook(file);
    }
```

# 2、ExcelHelper

```java
@Slf4j
public class ExcelHelper {
    private final static String XLS = "xls";
    private final static String XLSX = "xlsx";  
    
public static Workbook getWorkbook(MultipartFile file) {
        Workbook workbook = null;
        try {
            if (file != null) {
                 // 1.获得文件名 和 文件字节流
                String fileName = file.getOriginalFilename(); 
                InputStream is = file.getInputStream();  
              	//2. 根据流创建不同的WorkBook 
                if (fileName.toLowerCase().endsWith(XLS)) {//2003
                    
                    workbook = new HSSFWorkbook(is);
                    
                } else if (fileName.toLowerCase().endsWith(XLSX)) { // 2007
                    
                    workbook = new XSSFWorkbook(is);
                }
            } else {
                workbook = new HSSFWorkbook();
            }
        } catch (Exception e) {
            log.error("AVC曲线：获取Workbook对象出错{}", e);
            throw new RuntimeException(e);
        }
        return workbook;
    }
}
```

# 3、importMyNeed

```java
  public CommonResult importList(@RequestPart("file") MultipartFile file) {
      	//1.得到工作簿对象
        Workbook workbook = ExcelHelper.getWorkbook(file);
      
       //2.解析得到List集合
       List<Map<String, Object>> list = importMyNeed(workbook);
       
        //3.遍历查看获取情况
       list.forEach(System.out::println);
   }
```

- 从工作簿中获取表。调用表的获取所有物理行总数。执行遍历。

```java
  private List<Map<String, Object>> importMyNeed(Workbook workbook){
        List<Map<String, Object>> list = new ArrayList<>();  
        //1.第一个表
        Sheet sheet = workbook.getSheetAt(0);
       
       //2.表的物理行
       int rowNum = sheet.getPhysicalNumberOfRows();
       
      //3.遍历需要行的需要列
       for (int i = 4; i < rowNum-3; i++) {
            Map<String, Object> obj = new HashMap<>();
          	
            //3.1 获取需要行需要列的值
            String DQ   = this.getCellValue(sheet.getRow(i).getCell(3));
            String WGZDDL_ZAOFENG   = this.getCellValue(sheet.getRow(i).getCell(4));
            String WGZDDL_WUFENG  = this.getCellValue(sheet.getRow(i).getCell(7));
            String WGZDDL_WANFENG   = this.getCellValue(sheet.getRow(i).getCell(10));
			
            //3.2 放入容器
            obj.put("DQ",DQ);
            obj.put("WGZDDL_ZAOFENG", StringUtils.isBlank(WGZDDL_ZAOFENG) ? BigDecimal.ZERO :new BigDecimal(WGZDDL_ZAOFENG));
            obj.put("WGZDDL_WUFENG", StringUtils.isBlank(WGZDDL_WUFENG) ? BigDecimal.ZERO :new BigDecimal(WGZDDL_WUFENG));
            obj.put("WGZDDL_WANFENG", StringUtils.isBlank(WGZDDL_WANFENG) ? BigDecimal.ZERO :new BigDecimal(WGZDDL_WANFENG));

            list.add(obj);
        }
      
        return list;
    }
```

- 表有getRow(),获取行的方法。得到当前列Cell对象，类Cell对象有getStringCellValue()得到这个列的String类型的值。

```java
private String getCellValue(Cell cell){
        String cellValue = "";
        
     if (cell == null) {
            return cellValue;
        }
    
     cell.setCellType(CellType.STRING);
        cellValue = String.valueOf(cell.getStringCellValue());
        
    if (cellValue != null) {
            cellValue = ((String) cellValue).trim();
        }
    
        return cellValue;
    }
```

# 4、Some Method

```java
public class ExcelHelper {	
   
    //获取单元格的值
    public static Object getCellValue(Cell cell) {
            Object cellValue = "";
            if (cell == null) {
                return cellValue;
            }
            switch (cell.getCellTypeEnum()) { // 判断数据的类型
                case NUMERIC: // 数字
                    if (HSSFDateUtil.isCellDateFormatted(cell)) {
                        return cell.getDateCellValue();
                    }
                case FORMULA: // 公式
                    cellValue = cell.getNumericCellValue();
                    break;
                case STRING: // 字符串
                    cellValue = String.valueOf(cell.getStringCellValue());
                    break;
                case BOOLEAN: // Boolean
                    cellValue = cell.getBooleanCellValue();
                    break;
            }
            if (cellValue instanceof String) {
                cellValue = ((String) cellValue).trim();
            }
            return cellValue;
        }

	//设置单元格样式
    private static CellStyle getCellStyle(CellStyle cellStyle) {
            cellStyle.setBorderTop(BorderStyle.THIN); // 上边框
            cellStyle.setBorderBottom(BorderStyle.THIN); // 下边框
            cellStyle.setBorderLeft(BorderStyle.THIN); // 左边框
            cellStyle.setBorderRight(BorderStyle.THIN); // 右边框
            cellStyle.setTopBorderColor(IndexedColors.BLACK.getIndex()); // 上边框颜色
            cellStyle.setBottomBorderColor(IndexedColors.BLACK.getIndex()); // 下边框颜色
            cellStyle.setLeftBorderColor(IndexedColors.BLACK.getIndex()); // 左边框颜色
            cellStyle.setRightBorderColor(IndexedColors.BLACK.getIndex()); // 右边框颜色
            cellStyle.setVerticalAlignment(VerticalAlignment.CENTER); // 垂直居中
            cellStyle.setAlignment(HorizontalAlignment.CENTER); // 水平居中
             cellStyle.setWrapText(true); // 设置自动换行
            return cellStyle;
        }
    
    
    //下载Excel
    public static void downloadFile(HttpServletResponse response, String fileName, Workbook workbook) throws IOException {
            OutputStream outputStream = null;
            try {
                if (!fileName.endsWith(".xlsx") && !fileName.endsWith(".xls")) {
                    fileName = fileName + (workbook instanceof HSSFWorkbook ? ".xls" : ".xlsx");
                }
                fileName = new String(fileName.getBytes(StandardCharsets.UTF_8), StandardCharsets.ISO_8859_1);
                response.setHeader("content-type", "application/octet-stream");
                response.setHeader("Content-Disposition", "attachment;filename=" + fileName);
                response.addHeader("Pargam", "no-cache");
                response.addHeader("Cache-Control", "no-cache");

                outputStream = response.getOutputStream();
                workbook.write(outputStream);
                outputStream.flush();
            } finally {
                if (outputStream != null) {
                    try {
                        outputStream.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
}
```

