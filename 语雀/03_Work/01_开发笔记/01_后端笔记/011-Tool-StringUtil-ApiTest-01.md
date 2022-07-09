# Tool-StringUtils-ApiTest-01

# 1、isBlank()

```java
    @Test
    public void test_isBlank() {
        String str = null;
        String str1 = " ";
        String str2 = "";
        String str3 = "a ";


        Assertions.assertTrue(StringUtil.isBlank(str));
        Assertions.assertTrue(StringUtil.isBlank(str1));
        Assertions.assertTrue(StringUtil.isBlank(str2));
        Assertions.assertFalse(StringUtil.isBlank(str3));
    }
```

```java
    public static boolean isBlank(String str) {
        //1.遍历字符串每一个字符
        int length;
        if (str != null && (length = str.length()) != 0) {
            for (int i = 0; i < length; ++i) {
                //2.如果字符没有空字符，则返回false
                if (!Character.isWhitespace(str.charAt(i))) {
                    return false;
                }
            }
        }

        //3.否则返回ture
        return true;
    }
```

# 2、isNotBlank()

```java
    @Test
    public void test_isNotBlank() {
        String str = null;
        String str1 = " ";
        String str2 = "";
        String str3 = "a ";
        
        Assertions.assertFalse(StringUtil.isNotBlank(str));
        Assertions.assertFalse(StringUtil.isNotBlank(str1));
        Assertions.assertFalse(StringUtil.isNotBlank(str2));
        Assertions.assertTrue(StringUtil.isNotBlank(str3));

    }
```

```java
  public static boolean isNotBlank(String str) {
         //1.遍历字符串每一个字符
        int length;
        if (str != null && (length = str.length()) != 0) {
            for (int i = 0; i < length; ++i) {
                 //2.如果字符没有空字符，则返回ture
                if (!Character.isWhitespace(str.charAt(i))) {
                    return true;
                }
            }

        }
       //3.否则返回false
        return false;
    }
```

# 3、isNullOrEmpty()

```java
   @Test
    public void test_isNullOrEmpty() {
         String str1 = null;
         String str2 = "";
         String str3 = " ";

         Assertions.assertTrue(StringUtil.isNullOrEmpty(str1));
         Assertions.assertTrue(StringUtil.isNullOrEmpty(str2));
         Assertions.assertTrue(StringUtil.isNullOrEmpty(str3));

    }
```

```java
   public static boolean isNullOrEmpty(String s) {
        //1.字符串为空 或者为空串的情况下返回true
        return s == null || isBlank(s);
    }
```

# 4、equal()

```java
    @Test
    public void test_equal() {
        String str1 = "abc";
        String str2 = "abc";

        String str3 = "ABC";

        Assertions.assertTrue(StringUtil.equal(str1,str2,false));
        Assertions.assertTrue(StringUtil.equal(str1,str3,true));
    }
```

```java
    public static boolean equal(String s1, String s2, boolean p_ignoreCase) {
        //1.确保要比较相等的str1 str2 不为null
        if (s1 != null && s2 != null) {
            //2.根据 p_ignoreCase 来选择通过equalsIgnoreCase() equals()
            return p_ignoreCase ? s1.equalsIgnoreCase(s2) : s1.equals(s2);
        } else {
            return false;
        }
    }
```

# 5、isEmpty()

```java
    @Test
    public void test_isEmpty() {
        String str1 = null;
        String str2 = "";
        String str3 = " ";

        Assertions.assertTrue(StringUtil.isEmpty(str1));
        Assertions.assertTrue(StringUtil.isEmpty(str2));
        Assertions.assertFalse(StringUtil.isEmpty(str3));


    }
```

```java
  public static boolean isEmpty(String str) {
       // str 为 null 获取str 长度为0 都返回 true  
        return str == null || str.length() == 0;
    }
```

# 6、isNotEmpty()

```java
    @Test
    public void test_isNotEmpty() {
        String str1 = null;
        String str2 = "";
        String str3 = " ";

        Assertions.assertFalse(StringUtil.isNotEmpty(str1));
        Assertions.assertFalse(StringUtil.isNotEmpty(str2));
        Assertions.assertTrue(StringUtil.isNotEmpty(str3));
    }

```

```java
   public static boolean isNotEmpty(String str) {
        return str != null && str.length() > 0;
    }
```

