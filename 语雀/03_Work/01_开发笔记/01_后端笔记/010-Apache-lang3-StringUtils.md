# Apache-lang3-StringUtils

# 1、split() 分隔字符串 => 字符串数组 

```java
 @Test
    public void test_Split(){
        String str = "aa,bb,cc";
        String[] array = StringUtils.split(str,",");


        log.info("数组=> {}",Arrays.toString(array));
        log.info("数组长度 => {}",array.length);

        //18:01:15.867 [main] INFO com.wnx.mall.test6.BigDecimalTest - 数组=> [aa, bb, cc]

        //18:01:15.871 [main] INFO com.wnx.mall.test6.BigDecimalTest - 数组长度 => 3

    }
```

# 2、join()  字符串数组 => 分隔字符串

```java
    @Test
    public void test_Join(){
        List<String> list = new ArrayList<>();
        list.add("aa");
        list.add("bb");
        list.add("cc");

        String joinStr = StringUtils.join(list, ",");

        log.info("分隔字符串 => {}",joinStr);
        
        //18:04:11.136 [main] INFO com.wnx.mall.test6.BigDecimalTest - 分隔字符串=> aa,bb,cc

    }
```

# 3、isNotBlank()

要求字符串不为null,字符串不为空串"",字符串不为只有空格的字符串。

```java
    @Test
    public void stringUtils_isNotBlank(){
        String str = "";
        String str1 = null;
        String str2 = "     ";
        
        String str3 = " bb ";
        String str4 = "wangnaixing";
        
        System.out.println(StringUtils.isNotBlank(str)); //false
        System.out.println(StringUtils.isNotBlank(str1)); //false
        System.out.println(StringUtils.isNotBlank(str2)); //false
        
        System.out.println(StringUtils.isNotBlank(str3)); //true
        System.out.println(StringUtils.isNotBlank(str4)); //true
    }
```

