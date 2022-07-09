# Tool-RegexUtil

```java
/**
 * 常用正则表达式
 */
public class RegexUtil {

    /**
     * 匹配中文字符的正则表达式
     * [\u4E00-\u9FA5\\s]+ 多个汉字，包括空格
     * [\u4E00-\u9FA5]+ 多个汉字，不包括空格
     * [\u4E00-\u9FA5] 一个汉字
     */
    public static final String CHINESE_REGEX = "[\\u4E00-\\u9FA5\\\\s]+";

    /**
     * 匹配常规的ID，数字，字母，下划线
     */
    public static final String ID_REGEX = "^(?!_)(?!.*?_$)[a-zA-Z0-9_-]+$";
    /**
     * 匹配空白行的正则表达式
     */
    public static final String EMPTY_LINE_REGEX = "\\n\\s*\\r";
    /**
     * 匹配URL的正则表达式
     */
    public static final String URL_REGEX = "(?i)(http:|https:)//[^[\\u4e00-\\u9fa5]+";
    /**
     * 匹配邮箱的正则表达式
     */
    public static final String EMAIL_REGEX = "^([a-zA-Z0-9_\\-\\.]+)@((\\[[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\.)|(([a-zA-Z0-9\\-]+\\.)+))([a-zA-Z]{2,4}|[0-9]{1,3})(\\]?)$";
    /**
     * 匹配正整数
     */
    public static final String POSITIVE_NUM_REGEX = "^[1-9]\\d*$";
    /**
     * 匹配字母。数字，下划线，中文
     */
    public static final String CORRECT_INPUT = "^[0-9a-zA-Z_\u4e00-\u9FA5]+$";
    /**
     * 匹配12位昵称
     */
    public static final String CORRECT_NICK_INPUT = "^[0-9a-zA-Z\u4E00-\u9FA5_]{1,12}$";
    /**
     * 手机号码正则表达式
     */
    public static final String MOBILE = "^((13[0-9])|(15[^4,\\D])|(18[0,2-9])|(147)|(145)|(178))\\d{8}$";

    /**
     * UTC时间格式的正则表达式 "2019-10-10T01:43:11.000Z"
     */
    public static final String UTC_DATE_REGEX = "^\\d{4}(-)\\d{1,2}\\1\\d{1,2}T\\d{2}:\\d{2}:\\d{2}(.)\\d{3}(Z)";

    /**
     * 判断是否匹配正则表达式
     *
     * @param str        要匹配的字符串
     * @param patternStr 正则表达式
     * @return
     */
    public static Boolean isMatches(String str, String patternStr) {
        Pattern pattern = Pattern.compile(patternStr);
        Matcher matcher = pattern.matcher(str);
        return matcher.matches();
    }

}

```

```java
  private void requestBodyPretreatment(FormDataRequest dataRequest) {
        Map<String, List<Map<String, Object>>> formDataToUpdate = dataRequest.getFormDataToUpdate();
        formDataToUpdate.forEach((tableAlias, updateDataList) -> {
            updateDataList.forEach(dataMap -> {
                
                dataMap.forEach((filed, value) -> {
                    
          
                    if (Objects.nonNull(value) && RegexUtil.isMatches(String.valueOf(value), RegexUtil.UTC_DATE_REGEX)) {
                       
                        // 2019-10-10T01:43:11.000Z => 2019-10-10T01:43:11.000 UTC
                        String dataContent = String.valueOf(value).replace("Z", " UTC");
                   
                        try {
                            //返回 yyyy-MM-dd HH:mm:ss 格式日期字符串
                            Date date = (new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS Z")).parse(dataContent);
                            
                            dataMap.put(filed, (new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")).format(date));
                            
                        } catch (ParseException e) {
                            log.warn("时间格式化字符串：{}失败", value);
                        }
                    }
                });
            });
        });
    }
```

