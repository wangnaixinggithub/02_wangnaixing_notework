# Use-JDK-LocalDateApi

# 1、Create

```java
    @Test
    public void now(){
      
        //1.获取当前日期
        LocalDate now = LocalDate.now();
        
        //2.输出结果：2022-02-23
        System.out.println(now);
    }
```

```java
    @Test
    public void of(){
        int year = 1997;
        int month = 7;
        int day = 22;
       
        //1.获取指定日期
        LocalDate date = LocalDate.of(year, month, day);
        
        //2.输出结果：2022-07-22
        System.out.println(date);
    }
```

# 2、parse

```java
    @Test
    public void parse(){
        //dateStr => LocalDate
        String dateStr = "1997-07-22";
        LocalDate date = LocalDate.parse(dateStr);
    }
```

```java
  public static LocalDate parse(CharSequence text) {
      	//底层是使用默认DateTimeFormatter 为 ISO_LOCAL_DATE
        return parse(text, DateTimeFormatter.ISO_LOCAL_DATE);
    }
```



# 3、lengthOfYear

```java
   @Test
    public void lengthOfYear(){
        //1.获取当下日期
        LocalDate now = LocalDate.now();
        
        //2.算出日期所在年有多少天
        int daySum = now.lengthOfYear();
        
        System.out.println("这一日期所在年有多少天->"+daySum); //365天
    }
```

# 4、getXxx()

```java
    //getXxx():获取日期的信息
    @Test
    public void getInfo(){
        LocalDate now = LocalDate.of(2022,1,9);
        //1.获取年
        int year = now.getYear();
        
        //2.获取月
        Month month = now.getMonth();
        
        //3.获取月值
        int monthValue = now.getMonthValue();
        
        //4.一年的第几天
        int dayOfYear = now.getDayOfYear();
        
        //5.获取周几
        DayOfWeek dayOfWeek = now.getDayOfWeek();
        
        //6.获取月份字段枚举 根据这个枚举能得到月值
        int dayOfMonth = now.getDayOfMonth();

        System.out.println(year); //2022
        
        System.out.println(month + "-" + month.getValue()); // JANUARY - 1
        
        System.out.println(monthValue); // 1  1月
        
        System.out.println(dayOfYear); // 9 1年的第9天
        
        System.out.println(dayOfWeek); // SUNDAY 
        
        System.out.println(dayOfWeek.getValue()); //7

        System.out.println(dayOfMonth);  //9号
    }
```

# 5、plusXXX() And minusXXX()

```java
    //测试日期偏移量
    @Test
    public void plusOffSet(){
        LocalDate now = LocalDate.now();
        //1.在now日期下加1年
        LocalDate addOneYear = now.plusYears(1);
        
        //2.在now日期下加1月
        LocalDate addOneMonth = now.plusMonths(1);
        
    
        //3.在now日期下加1周
        LocalDate addOneWeek = now.plusWeeks(1);

        //4.在now日期下加1天
        LocalDate addOneDay = now.plusDays(1);
        


        System.out.println(addOneYear);
        System.out.println(addOneMonth);
        System.out.println(addOneDay);
        System.out.println(addOneWeek);

        System.out.println("-----------------------------");
        LocalDate delOneYear = now.minusYears(1);
        LocalDate delOneMonth = now.minusMonths(1);
        LocalDate delOneDay = now.minusDays(1);
        LocalDate delOneWeek = now.minusWeeks(1);


        System.out.println(delOneYear);
        System.out.println(delOneMonth);
        System.out.println(delOneDay);
        System.out.println(delOneWeek);

    }
```

# 6、isXXX()

```java

    @Test
    public void isXxx(){
        LocalDate now1 = LocalDate.of(2022,1,1);
        LocalDate now2 = LocalDate.of(2022, 1, 2);
        //1.日期之前
        boolean before = now1.isBefore(now2);
        
        //2.日期之后
        boolean after = now1.isAfter(now2);
        
        //3.日期相等
        boolean equal = now1.isEqual(now2);
        
        //4.闰年
        boolean leapYear = now1.isLeapYear();
    }

```

# 7、计算一年的第一个周一的办法

```java
   @Test
    public void test(){
        int year = 2022;
        //1.得到2022年1月1号
        LocalDate date = LocalDate.of(year, 1, 1);
       
        //2.获取是这周的第几天 
        int value = date.getDayOfWeek().getValue();
       
        //对1月一号进行加法，加到周末，再加1，就得到第一个周一了！！！
        LocalDate moneyDate = date.plusDays((7 - value) + 1);
       
        System.out.println(moneyDate);

    }
```

