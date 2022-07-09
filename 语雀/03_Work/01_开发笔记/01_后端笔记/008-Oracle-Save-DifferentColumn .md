# Oracle-Save-DifferentColumn 

# 1、Business Operation

- 1、假设第二次提交的数据从前端得到以Map的形式保存

- 2、假设第一次提交的数据已经被你通过SQL查询得到，以Map的形式保存。

```java
    private String findDifferentColumn(Map<String, Object> lastSbData, Map<String, Object> nowSbData) {
        String compareColumn = "RCSW_NUM,QSL_NUM,PJCL_NUM,SWBH_NUM,DYXSM,YCQJLL,RJHFDL_NUM,FDLL,ZXCL_NUM,YCRKLL,HSL_NUM,LYMPJYL,ZDCL_NUM,RMSW_NUM,FDST";
        List<String> differentColumnContainer = new ArrayList<>();
        
        //1.lastSbData 保存变换字段的key-value
        for (String key : compareColumn.split(",")) {
            lastSbData.remove(key, nowSbData.get(key));
        }
       
        //2.流中取出key
        lastSbData.keySet().stream()
                .filter(key -> (Arrays.asList(compareColumn.split(",")).contains(key)))
                .forEach(differentColumnContainer::add);
        
        //3.转成String
        return StringUtils.join(differentColumnContainer, ",");
    }

```

# 2、Map#remove()

> 移除Map中Entry 必须保证 =>key value 和remove() 形参一致， 才移除Map中Entry.

```java
public interface Map<K,V> {
    
    default boolean remove(Object key, Object value) {
            
         //1.根据key，获取现在Map中对应value
            Object curValue = get(key);
           
           //2.比较 curValue 和 value 
            if (!Objects.equals(curValue, value) ||
                (curValue == null && !containsKey(key))) {
                return false;
            }
        	
           //3.相等前提下，才remove key
            remove(key);
        
        	//4. 返回成功！
            return true;
        }
}        
```

# 3、Use Enum And Collectors.joining

```java
   private Map<String, Object> doUpdateSBHGDZDColumnValue(Map<String, Object> lastSbData, Map<String, Object> nowSbData) {
       //  String compareColumn = "RCSW_NUM,QSL_NUM,PJCL_NUM,SWBH_NUM,DYXSM,YCQJLL,RJHFDL_NUM,FDLL,ZXCL_NUM,YCRKLL,HSL_NUM,LYMPJYL,ZDCL_NUM,RMSW_NUM,FDST";
        String[] validationArray = StringUtils.split(SkyxjhTableEnum.SDRJHB.getValidation(), ",");

        //1.do For After: lastSbData 只存储改动字段的Entry
        for (String key : StringUtils.split(SkyxjhTableEnum.SDRJHB.getValidation(), ",")) {
            lastSbData.remove(key, nowSbData.get(key));
        }

        //2.把改动字段名以逗号分隔字符串的形式 记录下来。
        String SBHGDZD = lastSbData.keySet().stream()
                .filter(key -> Arrays.asList(validationArray).contains(key))
                .collect(Collectors.joining(","));

        //3.存储至表记录字段 SBHGDZD
        if (StringUtils.isNotBlank(SBHGDZD)) {
            nowSbData.put("SBHGDZD", SBHGDZD);
        }

        return nowSbData;
    }
```





