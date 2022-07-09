# Use-JDK-enum

# 1、Business

> 流程业务：每一个人工活动都有唯一活动标识和活动名称。这时候，我们一般会定义枚举类，遍历我们在代码中对每一个流程节点进行数据化。

<img src="C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220419145311238.png" alt="image-20220419145311238" style="zoom: 67%;" />

```java
@Getter
@Setter
public enum AzsbjxlcProcessEnum {

    AZSBJXSQLC("安自设备检修流程"),
    DCSQ("电厂申请"),
    ZDAZZZSHBBZ("中调安自专责审核并编制"),
    DDAZZZSHBBZ("地调安自专责审核并编制"),
    AZZGJH("安自主管校核"),
    YXZZSH("方式专责审核"),
    DDZZSH("调度专责审核"),
    JBZZSH("继保专责审核"),
    ZDHZZSH("自动化专责审核"),
    TXZZSH("地调通信专责审核"),
    FSZGSP("方式部长审批"),
    ZXLDSP("中心领导审批"),
    AZZZQR("安自专责确认"),
    DDZX("调度台执行"),
    TZDC("通知电厂"),
    BACX("备案查询"),
    AZJXZZYQ("安自检修专责延期"),
    GD("归档");

    String atvdName;
    

    AzsbjxlcProcessEnum(String atvdName){
        this.atvdName = atvdName;
    }
    public String getAtvdName() {
        return atvdName;
    }

    public void setAtvdName(String atvdName) {
        this.atvdName = atvdName;
    }
	
  
    public static AzsbjxlcProcessEnum getValueByAtvdId(String atvdId){
        for(AzsbjxlcProcessEnum processEnum : AzsbjxlcProcessEnum.values()){
            if(processEnum.name().equals(atvdId)){
                return processEnum;
            }
        }
        return null;
    }
}

```

# 2、AzsbjxlcProcessEnum.XXX

<img src="C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220419150002941.png" alt="image-20220419150002941" style="zoom:50%;" />

```java
 @Test
    public void test(){
        //返回的是枚举类实例
        AzsbjxlcProcessEnum dcsq = AzsbjxlcProcessEnum.DCSQ;
    }
```

# 3、Use Getter Fetch Prop

```java
   @Test
    public void test_enum_getAtvdName(){
        String atvdName = AzsbjxlcProcessEnum.DCSQ.getAtvdName();
        System.out.println(atvdName);       //电厂申请
    }
```

# 4、values() => GET enumArray

```java
   @Test
    public void test2_values_method(){
        AzsbjxlcProcessEnum[] enumArray = AzsbjxlcProcessEnum.values();
       
        for (AzsbjxlcProcessEnum processEnum : enumArray) {
            System.out.println(processEnum);
        }
        
        //AZSBJXSQLC
        //DCSQ
        //ZDAZZZSHBBZ
        //DDAZZZSHBBZ
        //AZZGJH
        //YXZZSH
        //DDZZSH
        //JBZZSH
        //ZDHZZSH
        //TXZZSH
        //FSZGSP
        //ZXLDSP
        //AZZZQR
        //DDZX
        //TZDC
        //BACX
        //AZJXZZYQ
        //GD
    }
```

# 5、name() 

```java
 @Test
    public void test2_values_method(){
        AzsbjxlcProcessEnum[] enumArray = AzsbjxlcProcessEnum.values();
        for (AzsbjxlcProcessEnum processEnum : enumArray) {
            
            System.out.println(processEnum);
            System.out.println(processEnum.name());
            
            //toString() 和 name()  等效！
           

        }

    }

        //AZSBJXSQLC
        //DCSQ
        //ZDAZZZSHBBZ
        //DDAZZZSHBBZ
        //AZZGJH
        //YXZZSH
        //DDZZSH
        //JBZZSH
        //ZDHZZSH
        //TXZZSH
        //FSZGSP
        //ZXLDSP
        //AZZZQR
        //DDZX
        //TZDC
        //BACX
        //AZJXZZYQ
        //GD

```

