# JDK-enum-Use-02

# 1、Filter MyNeed Enum

```java
//（地调）调度执行 必填的主表字段枚举类
public enum AzsbjxlcDdzxDDColumnEnum {
    
	SJDDCZLSQPF_SJ("地调操作记录【上级调度操作令申请批复时间】未填写！", "待开工"),
    SJDDCZLSQPF_CZFZR("地调操作记录【厂站负责人】未填写！", "待开工"),
    SJDDCZLSQPF_DDFZR("地调操作记录【上级调度负责人】未填写！", "待开工"),
    XK_TDKS_SJ("申请许可【执行二次安全措施开始时间】未填写！", "待开工"),
    XK_TDKS_SLR("申请许可【受令人】未填写！", "待开工"),
    XK_TDKS_DDY("申请许可【调度员】未填写！", "待开工"),
    
    
    XK_TDJS_SJ("申请许可【执行二次安全措施结束时间】未填写！", "开工操作任务中"),
    XK_TDJS_HLR("申请许可【回令人】未填写！", "开工操作任务中"),
    XK_TDJS_DDY("申请许可【调度员】未填写！", "开工操作任务中"),
    

    ZJ_JBFDTJ("申请终结【具备复电条件】未选择！", "正在检修中"),
    ZJ_FDKS_SJ("申请终结【恢复执行二次安全措施开始时间】未填写！", "正在检修中"),
    ZJ_FDKS_SLR("申请终结【受令人】未填写！", "正在检修中"),
    ZJ_FDKS_DDY("申请终结【调度员】未填写！", "正在检修中"),
    
    
    ZJ_FDJS_SJ("申请终结【恢复执行二次安全措施结束时间】未填写！", "完工操作任务中"),
    ZJ_FDJS_HLR("申请终结【回令人】未填写！", "完工操作任务中"),
    ZJ_FDJS_DDY("申请终结【调度员】未填写！", "完工操作任务中");

	

	private String warnningText;

    private String ddzxlzzt;
    

    AzsbjxlcDdzxDDColumnEnum(String warnningText, String ddzxlzzt) {
        this.warnningText = warnningText;
        this.ddzxlzzt = ddzxlzzt;
    }
    
  
   public String getWarnningText() {
        return warnningText;
    }

    public String getDdzxlzzt() {
        return ddzxlzzt;
    }
}
```

```java
public enum AzsbjxlcDdzxDDColumnEnum {

    public static List<AzsbjxlcDdzxDDColumnEnum> getListByDdzxlzzt(String ddzxlzzt) {
       
        if (StringUtils.isBlank(ddzxlzzt)) {
            return Collections.EMPTY_LIST;
        }

        return Arrays.stream(AzsbjxlcDdzxDDColumnEnum.values())
            .filter(x -> ddzxlzzt.equals(x.ddzxlzzt))
            .collect(Collectors.toList());
    }
  }  
```

> 比如我们筛选出来`ddzxlzzt`为"待开工"的。

```java
@Test
public void test2(){
    List<AzsbjxlcDdzxDDColumnEnum> list = AzsbjxlcDdzxDDColumnEnum.getListByDdzxlzzt("待开工");
    list.forEach(System.out::println);
    //SJDDCZLSQPF_SJ
    //SJDDCZLSQPF_CZFZR
    //SJDDCZLSQPF_DDFZR
    //XK_TDKS_SJ
    //XK_TDKS_SLR
    //XK_TDKS_DDY
}
```

> 比如我们筛选出`ddzxlzzt`为"开工操作任务中"

```java
 @Test
    public void test3(){
        List<AzsbjxlcDdzxDDColumnEnum> list = AzsbjxlcDdzxDDColumnEnum.getListByDdzxlzzt("开工操作任务中");
        list.forEach(System.out::println);
        //XK_TDJS_SJ
        //XK_TDJS_HLR
        //XK_TDJS_DDY
    }
```

> 比如我们筛选出`ddzxlzzt`为"正在检修中"

```java
 @Test
    public void test4(){
        List<AzsbjxlcDdzxDDColumnEnum> list = AzsbjxlcDdzxDDColumnEnum.getListByDdzxlzzt("正在检修中");
        list.forEach(System.out::println);
        //ZJ_JBFDTJ
        //ZJ_FDKS_SJ
        //ZJ_FDKS_SLR
        //ZJ_FDKS_DD
    }
```

> 比如我们筛选出`ddzxlzzt`为"完工操作任务中"

```java
    @Test
    public void test5(){
        List<AzsbjxlcDdzxDDColumnEnum> list = AzsbjxlcDdzxDDColumnEnum.getListByDdzxlzzt("完工操作任务中");
        list.forEach(System.out::println);
        //ZJ_FDJS_SJ
        //ZJ_FDJS_HLR
        //ZJ_FDJS_DDY
    }
```

## 2、primaryData-Table-Validation

- 写一个私有方法，把主表数据和`ddzxlzzt`字段传入。

```java
  private ResponseResult validatePrimaryDataByDdzxlzzt(Map<String, Object> primaryData, String ddzxlzzt){
        
      String message = "保存失败，%s";
      
      //1.获取需要校验的枚举
      List<AzsbjxlcDdzxDDColumnEnum> list = AzsbjxlcDdzxDDColumnEnum.getListByDdzxlzzt(ddzxlzzt);
     
      for(AzsbjxlcDdzxDDColumnEnum e : list){
           
          //2.值判Null校验
            Object value = primaryData.get(e.name());         
            if (value == null || "".equals(value)) {
                return ResponseResult.failedResult(String.format(message, e.getWarnningText()));
            }
          
        }
  
        return ResponseResult.successResult();
    }
```

