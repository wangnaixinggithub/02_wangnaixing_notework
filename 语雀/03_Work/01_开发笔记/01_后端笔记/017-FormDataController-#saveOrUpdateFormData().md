# FormDataController-`#saveOrUpdateFormData()`

# 1、FormDataRequest

```java
@JsonInclude(JsonInclude.Include.NON_NULL)
@Data
public class FormDataRequest {
	
    //操作类型 create  Or update
    private String operation;

    //旧数据
    private Map<String, List<Map<String, Object>>> formData;

	//新数据
    private Map<String, List<Map<String, Object>>> formDataToUpdate;

   	//附加属性
    private Map<String, String> extensions;

  	//记录变化控制
    private Map<String, Boolean> recordChangesSwitch;

}

```

# 2、FormDataController

```java
@Slf4j
@Api(tags = "表单统一数据接口API")
@RestController
@RequestMapping(path = "/api/v1/form-data/")
public class FormDataController {
    
	@ResponseBody
    @PostMapping("{formModelId}")
    public CommonDataVO saveOrUpdateFormData(@PathVariable String formModelId,
                                             @RequestBody FormDataRequest dataRequest,
                                             HttpServletRequest servletRequest) {
        //1.formModelId：流程模型ID
       
        //2.FormDataRequest：Form数据请求
        
        //3.HttpServletRequest 请求
        
    }

}
```

# 3、`#saveOrUpdateFormData()`

```java
public class FormDataController {
    
	public CommonDataVO saveOrUpdateFormData(@PathVariable String formModelId,
                                             @RequestBody FormDataRequest dataRequest,
                                             HttpServletRequest servletRequest) {
        
        //1.预处理请求body
        requestBodyPretreatment(dataRequest);
        
      	//2.根据Form模型ID，解析JSON，得到FormDataModel对象。
        String userId = userService.getUserId();
        FormDataModel formDataModel = formDataService.parseFormDataModel(userId, formModelId);
        
        //3.执行新增或更新操作
        String primaryKey;
        if (StringUtils.equals(dataRequest.getOperation(), "create")) {
            log.info("操作类型是create，开始新增表单数据");
            
            primaryKey = formDataService.createFormData(dataRequest.getFormDataToUpdate(), formDataModel);
        
        } else {
            log.info("操作类型是update，开始更新表单数据");
            
            String originalRequest = null;
            originalRequest = objectMapper.writeValueAsString(dataRequest);
            formDataService.updateFormData(dataRequest.getFormDataToUpdate(), dataRequest.getFormData(), formDataModel);
            
            primaryKey = "更新成功";
       
            FormDataRequest originalDataRequest = objectMapper.readValue(originalRequest, FormDataRequest.class);
            
            formDataService.saveUpdateRecord(userId, formDataModel, originalDataRequest, dataRequest.getRecordChangesSwitch(), IPAddressUtil.getIPAddress(servletRequest));
           
        }
        
        //4.拼装成CommonDataVo 返回。
        CommonDataVO commonDataVO = new CommonDataVO();
        commonDataVO.setData(primaryKey);
        
        return commonDataVO;

        
    }
}
```

# 4、FormDataService

```java
public interface FormDataService {
	 //通过表单模型ID，得到表单模型的配置项
	 FormDataModel parseFormDataModel(String userId, String formModelId);
  	
    // 更新表单数据
    String createFormData(Map<String, List<Map<String, Object>>> formDataToCreate, FormDataModel formDataModel);
    
    //更新表单数据
    void updateFormData(Map<String, List<Map<String, Object>>> formDataToUpdate, Map<String, List<Map<String, Object>>> formData, FormDataModel formDataModel);
    
}
```

# 5、FormDataModelHelper

```java
public class FormDataModelHelper {
	public class FormDataModelHelper {

	//别名和clsId的映射map
    @Getter
    private Map<String, String> tableAliasClsIdMap;

	//别名和关联属性的映射map
    private Map<String, List<FormDataLinkProperties>> tableAliasLinkMap;

 	//别名和主键Column的映射map
    private Map<String, String> tableAliasPKColumnMap;

    public FormDataModelHelper(FormDataModel dataModel) {
        Map<String, String> clsIdMap = new HashMap<>();
        Map<String, List<FormDataLinkProperties>> linkPropertiesMap = new HashMap<>();
        Map<String, String> pkColumnMap = new HashMap<>();

        // 获取主表单的clsID
        String primaryTableAlias = getPrimaryTableAlias(dataModel);
        clsIdMap.put(primaryTableAlias, readClsIdFromModel(dataModel));

        // 获取主表单的主键Column
        if (StringUtils.isNotBlank(dataModel.getPrimaryKey())) {
            pkColumnMap.put(primaryTableAlias, dataModel.getPrimaryKey());
        } else {
            pkColumnMap.put(primaryTableAlias, "ID");
        }

        // 解析子表单的相关属性
        if (CollectionUtils.isNotEmpty(dataModel.getChildren())) {
            for (FormDataModel modelChild : dataModel.getChildren()) {
                clsIdMap.put(modelChild.getTableAlias(), readClsIdFromModel(modelChild));

                if (CollectionUtils.isNotEmpty(modelChild.getLinkProperties())) {
                    linkPropertiesMap.put(modelChild.getTableAlias(), modelChild.getLinkProperties());
                }

                if (StringUtils.isNotBlank(modelChild.getPrimaryKey())) {
                    pkColumnMap.put(modelChild.getTableAlias(), modelChild.getPrimaryKey());
                } else {
                    pkColumnMap.put(modelChild.getTableAlias(), "ID");
                }
            }
        }

        this.tableAliasClsIdMap = clsIdMap;
        this.tableAliasLinkMap = linkPropertiesMap;
        this.tableAliasPKColumnMap = pkColumnMap;
    }

	//从formDataModel读取clsId信息
    private String readClsIdFromModel(FormDataModel formDataModel) {
        if (StringUtils.isNotBlank(formDataModel.getClsId())) {
            return formDataModel.getClsId();
        }

        ClassControlDao classControlDao = PXBeanFactory.getBean(ClassControlDao.class);
        DataClass dataClass = classControlDao.getDataClassByTableAndSchema(formDataModel.getTableName(), formDataModel.getSchema());
        return dataClass.getId();
    }

	//根据别名获取clsId
    public String getClsId(String tableAlias) {
        if (Objects.isNull(tableAliasClsIdMap) || tableAliasClsIdMap.isEmpty()) {
            return null;
        }
        return tableAliasClsIdMap.get(tableAlias);
    }

	//根据别名获取子表单和主表单的Link关系
    public List<FormDataLinkProperties> getLinkInfo(String tableAlias) {
        if (Objects.isNull(tableAliasLinkMap) || tableAliasLinkMap.isEmpty()) {
            return null;
        }
        return tableAliasLinkMap.get(tableAlias);
    }

	//根据别名获取主键的column name
    public String getPKColumn(String tableAlias) {
        if (Objects.isNull(tableAliasPKColumnMap) || tableAliasPKColumnMap.isEmpty()) {
            return "ID";
        }
        return tableAliasPKColumnMap.get(tableAlias);
    }
        
	// 获取主表单的tableAlias
    public final String getPrimaryTableAlias(FormDataModel dataModel) {
        return StringUtils.isBlank(dataModel.getTableAlias()) ? FormDataConstant.PRIMARY_TABLE_ALIAS : dataModel.getTableAlias();
    }



}
```



# 6、FormDataServiceImpl#createFormData()

```java
    @Override
    @Transactional(rollbackFor = Exception.class)
    public String createFormData(Map<String, List<Map<String, Object>>> formDataToCreate, FormDataModel formDataModel) {
        FormDataModelHelper modelHelper = new FormDataModelHelper(formDataModel);

        // 保存主表单数据
        String primaryAlias = modelHelper.getPrimaryTableAlias(formDataModel);
        List<Map<String, Object>> primaryTableData = formDataToCreate.get(primaryAlias);
       
        if (CollectionUtils.isEmpty(primaryTableData)) {
            log.error("数据中不存在主表单的数据，无法进行后续保存");
            ErrorInfo errorInfo = new ErrorInfo(ErrorCode.invalid_request.name(), ErrorDescription.INVALID_REQUEST);
            throw new WebServerException(HttpStatus.BAD_REQUEST, errorInfo, "数据中不存在主表单的数据，无法进行后续保存");
        }
       
        Map<String, Object> primaryTable = primaryTableData.get(0);
       
        Serializable primaryKey = unifiedDataService.createObject(modelHelper.getClsId(primaryAlias), primaryTable);
       
        log.info("保存主表单成功，返回的主键为:{}", primaryKey);
       
        if (StringUtils.isNotBlank(formDataModel.getPrimaryKey())) {
            primaryTable.put(formDataModel.getPrimaryKey(), primaryKey);
        } else {
            primaryTable.put(modelHelper.getPKColumn(primaryAlias), primaryKey);
        }
       
        formDataToCreate.remove(primaryAlias);

        // 开始保存子表单数据
        for (String tableAlias : modelHelper.getTableAliasClsIdMap().keySet()) {
            List<Map<String, Object>> subTableData = formDataToCreate.get(tableAlias);
           
            if (CollectionUtils.isEmpty(subTableData)) {
                continue;
            }
          
            for (Map<String, Object> subTable : subTableData) {
                assembleSubTable(subTable, primaryTable, modelHelper.getLinkInfo(tableAlias));
               
                Serializable subTablePK = unifiedDataService.createObject(modelHelper.getClsId(tableAlias), subTable);
                log.info("保存子表单成功，返回的主键为:{}", subTablePK);
            }
        }
        log.info("子表单保存结束");

        return primaryKey.toString();
    }

```

# 7、FormDataServiceImpl#updateFormData()

```java
   @Override
    public void updateFormData(Map<String, List<Map<String, Object>>> formDataToUpdate, Map<String, List<Map<String, Object>>> formData, FormDataModel formDataModel) {
        FormDataModelHelper modelHelper = new FormDataModelHelper(formDataModel);

        // 开始获取有变更的数据
        log.debug("对比原数据和更新数据");
        Map<String, List<Map<String, Object>>> diffFormData = compareFormData(modelHelper, formDataToUpdate, formData);

        // 开始从数据库对比有变更的数据是否已被修改
        log.debug("和数据库对比有变更的数据是否已被修改");
        compareFormDataWithDB(modelHelper, diffFormData);

        // 对比需要删除的子表单数据并进行删除
        log.debug("删除更新数据中不存在的子表单数据");
        deleteSubTableDiffRecords(formDataModel, modelHelper, formDataToUpdate, formData);

        // 更新主表单数据
        Map<String, Object> primaryTable = null;
        String primaryAlias = modelHelper.getPrimaryTableAlias(formDataModel);
        List<Map<String, Object>> primaryTableData = formDataToUpdate.get(primaryAlias);
        if (CollectionUtils.isNotEmpty(primaryTableData)) {
            primaryTable = primaryTableData.get(0);
        }

        String primaryColumn = modelHelper.getPKColumn(primaryAlias);
        if (CollectionUtils.isEmpty(diffFormData.get(primaryAlias))) {
            log.info("主表单数据没有变更，不进行数据库操作");
        } else {
            Map<String, Object> primaryDiffData = diffFormData.get(primaryAlias).get(0);
            Map<String, Object> primaryTableToUpdate = Maps.newHashMapWithExpectedSize(primaryDiffData.keySet().size() + 1);
            for (String updateColumn : primaryDiffData.keySet()) {
                primaryTableToUpdate.put(updateColumn, primaryTable.get(updateColumn));
            }
            primaryTableToUpdate.put(primaryColumn, primaryTable.get(primaryColumn));
            Serializable updateResult = unifiedDataService.createOrSaveObject(modelHelper.getClsId(primaryAlias), primaryTableToUpdate);
            log.info("更新主表单[{}]成功，返回的结果为:{}", modelHelper.getClsId(primaryAlias), updateResult);
        }
        formDataToUpdate.remove(primaryAlias);

        // 开始更新子表单数据
        for (String tableAlias : formDataToUpdate.keySet()) {
            List<Map<String, Object>> formDataList = formDataToUpdate.get(tableAlias);
            for (Map<String, Object> subTable : formDataList) {
                assembleSubTable(subTable, primaryTable, modelHelper.getLinkInfo(tableAlias));
                String subUpdateResult = unifiedDataService.createOrSaveObject(modelHelper.getClsId(tableAlias), subTable);
                log.info("更新子表单[{}]成功，返回的结果为:{}", modelHelper.getClsId(tableAlias), subUpdateResult);
            }
        }
        log.info("表单更新完成");
    }
```

