# SpringBoot-Merge-Easypoi



```xml
<dependency>
    <groupId>cn.afterturn</groupId>
    <artifactId>easypoi-spring-boot-starter</artifactId>
    <version>4.4.0</version>
</dependency>
```

# 1、 @Excel

```java
@Data
public class Member implements Serializable {

    /**
     * 自增主键
     */
    @Excel(name = "自增主键")
    private Long id;
    /**
     * 账号
     */
    @Excel(name = "账号",width = 20,needMerge = true)
    private String username;
    /**
     * 密码
     */
    private String password;
    /**
     * 昵称
     */
    @Excel(name = "昵称",width = 10)
    private String nickname;
    /**
     * 生日
     */
    @Excel(name = "出生日期", width = 20, format = "yyyy/MM/dd")
    private Date birthday;
    /**
     * 电话
     */
    @Excel(name = "电话",width = 20,needMerge = true,desensitizationRule = "3_4")
    private String phone;

    /**
     * 头像
     */
    private String icon;
    /**
     * 性别
     */
    @Excel(name = "性别",width = 10,replace = {"男_0","女_1"})
    private Integer gender;

}

```

- 在此我们就可以看到`EasyPoi`的核心注解`@Excel`，通过在对象上添加`@Excel`注解，可以将对象信息直接导出到Excel中去，下面对注解中的属性做个介绍；
- - `name`：Excel中的列名；
  - `width`：指定列的宽度；
  - `needMerge`：是否需要纵向合并单元格；
  - `format`：当属性为时间类型时，设置时间的导出导出格式；
  - `desensitizationRule`：数据脱敏处理，`3_4`表示只显示字符串的前`3`位和后`4`位，其他为`*`号；
  - `replace`：对属性进行替换；
  - `suffix`：对数据添加后缀。
- 接下来我们在Controller中添加一个接口，用于导出会员列表到Excel，具体代码如下；

# 2、 PoiBaseView.render()

```java
@RestController
@Api(tags = "EasyPoiController", description = "EasyPoi导入导出测试")
@RequestMapping("/easyPoi")
public class EasyPoiController {

    @ApiOperation(value = "导出会员列表Excel")
    @GetMapping(value = "/exportMemberList")
    public void exportMemberList(HttpServletRequest request, HttpServletResponse response) {
        
        //假设这是你需要导出的成员列表
        List<Member> memberList = LocalJsonUtil.getListFromJson("json/members.json", Member.class);
         Map<String,Object> paramMap = new HashMap<>();
        
        
        //1、创建输出参数对象
        ExportParams exportParams = new ExportParams("会员列表", "会员列表", ExcelType.XSSF);
       

        //2、设置FileName
        paramMap.put(NormalExcelConstants.FILE_NAME, "memberList");
        
        //3、设置DataList
        paramMap.put(NormalExcelConstants.DATA_LIST, memberList);
        
        //4、设置List POJO
        paramMap.put(NormalExcelConstants.CLASS, Member.class);
        
   		//5.设置输出参数对象
        paramMap.put(NormalExcelConstants.PARAMS, exportParams);


        //6、执行render
        PoiBaseView.render(paramMap, request, response, NormalExcelConstants.EASYPOI_EXCEL_VIEW);

    }
    
}

```

![](D:\my_notebook\SpringBoot实现Excel导入导出，好用到爆，POI可以扔掉了！\QQ截图20211118225449.png)

# 3、ExcelImportUtil.importExcel()

```java
        @ApiOperation("从Excel导入会员列表")
        @PostMapping(value = "/importMemberList")
        public CommonResult<Object> importMemberList(@RequestPart("file") MultipartFile file) {
            
            //1.创建导入参数对象
            ImportParams params = new ImportParams();
		
            params.setTitleRows(1);
            params.setHeadRows(1);
            


            //2.解析Excel文件流得到 List
            List<Member> list = ExcelImportUtil.importExcel(file.getInputStream(), Member.class, params);

            return CommonResult.success(list);
           
        }
```

```java
@Data
public class Product implements Serializable {
    /**
     * ID
     */
    @Excel(name = "ID", width = 10)
    private Long id;
    /**
     * 商品SN
     */
    @Excel(name = "商品SN", width = 20)
    private String productSn;
    /**
     * 商品名称
     */
    @Excel(name = "商品名称", width = 20)
    private String name;
    /**
     * 商品副标题
     */
    @Excel(name = "商品副标题", width = 30)
    private String subTitle;
    /**
     * 品牌名称
     */
    @Excel(name = "品牌名称", width = 20)
    private String brandName;
    /**
     * 商品价格
     */
    @Excel(name = "商品价格", width = 10)
    private BigDecimal price;
    /**
     * 购买数量
     */
    @Excel(name = "购买数量", width = 10, suffix = "件")
    private Integer count;
}

```

# 4、@ExcelEntity  @ExcelCollection

- 然后添加订单对象`Order`，订单和会员是一对一关系，使用`@ExcelEntity`注解表示，订单和商品是一对多关系，使用`@ExcelCollection`注解表示，`Order`就是我们需要导出的嵌套订单数据；

```java

@Data
public class Order implements Serializable {
    /**
     * ID
     */
    @Excel(name = "ID", width = 10,needMerge = true)
    private Long id;
    /**
     * 订单号
     */
    @Excel(name = "订单号", width = 20,needMerge = true)
    private String orderSn;
    /**
     * 创建时间
     */
    @Excel(name = "创建时间", width = 20, format = "yyyy-MM-dd HH:mm:ss",needMerge = true)
    private Date createTime;
    /**
     * 收货地址
     */
    @Excel(name = "收货地址", width = 20,needMerge = true )
    private String receiverAddress;
    /**
     * 会员信息
     */
    @ExcelEntity(name = "会员信息")
    private Member member;
    /**
     * 商品列表
     */
    @ExcelCollection(name = "商品列表")
    private List<Product> productList;
}

```

- 接下来在Controller中添加导出订单列表的接口，由于有些会员信息我们不需要导出，可以调用`ExportParams`中的`setExclusions`方法排除掉；

```java
 @ApiOperation(value = "导出订单列表Excel")
    @RequestMapping(value = "/exportOrderList", method = RequestMethod.GET)
    public void exportOrderList(HttpServletRequest request,
                                HttpServletResponse response) {
         //假设这是你需要导出的订单列表
        List<Order> orderList = getOrderList();
    	Map<String,Object> paramMap = new HashMap<>();	    
      
        //1、创建导出参数对象
        ExportParams params = new ExportParams("订单列表", "订单列表", ExcelType.XSSF);
    
        //2、导出时排除一些字段
        params.setExclusions(new String[]{"ID", "出生日期", "性别"});
		
        //3.设置DataList
        map.put(NormalExcelConstants.DATA_LIST, orderList);
      
        //4.设置List POJO
        map.put(NormalExcelConstants.CLASS, Order.class);
        
        //5.设置导出参数对象
        map.put(NormalExcelConstants.PARAMS, params);
        
        //6.设置 fileName
        map.put(NormalExcelConstants.FILE_NAME, "orderList");
        
        //7.执行render
        PoiBaseView.render(map, request, response, NormalExcelConstants.EASYPOI_EXCEL_VIEW);
    
    }


    private List<Order> getOrderList() {

        List<Order> orderList = LocalJsonUtil.getListFromJson("json/orders.json", Order.class);
        List<Product> productList = LocalJsonUtil.getListFromJson("json/products.json", Product.class);
        List<Member> memberList = LocalJsonUtil.getListFromJson("json/members.json", Member.class);

        for (int i = 0; i < orderList.size(); i++) {
            Order order = orderList.get(i);
            order.setMember(memberList.get(i));
            order.setProductList(productList);
        }
        return orderList;
    }
```

- 下载完成后，查看下文件，`EasyPoi`导出复杂的`Excel`也是很简单的！

![](D:\my_notebook\SpringBoot实现Excel导入导出，好用到爆，POI可以扔掉了！\QQ截图20211118233044.png)

# 5、 extends ExcelDataHandlerDefaultImpl

> 如果你想对导出字段进行一些自定义处理，`EasyPoi`也是支持的，比如在会员信息中，如果用户没有设置昵称，我们添加下`暂未设置`信息。

- 我们需要添加一个处理器继承默认的`ExcelDataHandlerDefaultImpl`类，然后在`exportHandler`方法中实现自定义处理逻辑；

```java

public class MemberExcelDataHandler extends ExcelDataHandlerDefaultImpl<Member> {

    @Override
    public Object exportHandler(Member obj, String name, Object value) {
        if("昵称".equals(name)){
            String emptyValue = "暂未设置";
            
            //昵称值为null的情况
            if(value==null){
                return super.exportHandler(obj,name,emptyValue);
            }
            
            //昵称值为 ”“的情况
            if(value instanceof String && StrUtil.isBlank((String) value)){
                return super.exportHandler(obj,name,emptyValue);
            }
            
        }
        
        return super.exportHandler(obj, name, value);
    }

    @Override
    public Object importHandler(Member obj, String name, Object value) {
        return super.importHandler(obj, name, value);
    }
}
```

- 然后修改Controller中的接口，调用`MemberExcelDataHandler`处理器的`setNeedHandlerFields`设置需要自定义处理的字段，并调用`ExportParams`的`setDataHandler`设置自定义处理器；

```java
    @ApiOperation(value = "导出会员列表Excel")
    @GetMapping(value = "/exportMemberList")
    public void exportMemberList(HttpServletRequest request, HttpServletResponse response) {
        List<Member> memberList = LocalJsonUtil.getListFromJson("json/members.json", Member.class);
        Map<String,Object> paramMap = new HashMap<>();
        paramMap.put(NormalExcelConstants.FILE_NAME, "memberList");
        paramMap.put(NormalExcelConstants.DATA_LIST, memberList);
        paramMap.put(NormalExcelConstants.CLASS, Member.class);
        ExportParams exportParams = new ExportParams("会员列表", "会员列表", ExcelType.XSSF);
        
        //自定义Excel数据处理器
        MemberExcelDataHandler handler = new MemberExcelDataHandler();
        handler.setNeedHandlerFields(new String[]{"昵称"});
        exportParams.setDataHandler(handler);
        
        paramMap.put(NormalExcelConstants.PARAMS, exportParams);


        //执行导出下载excel
        PoiBaseView.render(paramMap, request, response, NormalExcelConstants.EASYPOI_EXCEL_VIEW);

    }
```

- 再次调用导出接口，我们可以发现昵称已经添加默认设置了。

![](D:\my_notebook\SpringBoot实现Excel导入导出，好用到爆，POI可以扔掉了！\QQ截图20211118234246.png)





