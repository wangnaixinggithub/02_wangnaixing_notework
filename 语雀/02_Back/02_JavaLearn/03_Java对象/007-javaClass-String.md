# javaClass-String

# 1、substring()

- 得到字符串的子串。

```java
public class StringApi {
    public static void main(String[] args) {
        String str1 = "1234wangnaixing56789";
        //提出字母
        System.out.println(str1.substring(4,15));//wangnaixing
    }
}

```

# 2、repeat()

- 让一个字符串重复输入

```java
public class StringApi {
    public static void main(String[] args) {
        String name = "wangnaixing";
        String repeat = name.repeat(3);
        System.out.println(repeat);//wangnaixingwangnaixingwangnaixing
    }
}

```

# 3、equals（）

- 判断两个字符串是否相等

```java
public class StringApi {
    
    public static void main(String[] args) {
    
        String x1 = "abc";
        String x2 = "abc";
        
        System.out.println(x1.equals(x2));//true
    }
}

```

# 4、""

- 空串就是长度为0的字符串

```java
public class StringApi {
    
    public static void main(String[] args) {
    
        String x1 = "";//这是一个空串
        
        System.out.println(x1.length()); //0
    }
}

```

# 5、isNotBlank()

```java
public class StringApi {
    public static void main(String[] args) {
        
        String x1 = "";//这是一个空串
        
        if (x1!=null && x1.length() > 0){
            
            //判定字符串既不是null可不是空串的逻辑
        }
    }
}

```

# 6、charAt()

- 根据索引得到字符

```java
public class StringApi {
    public static void main(String[] args) {
    
        String x1 = "abcdefgh1jilif";
        
        char c = x1.charAt(3);
        
        System.out.println(c);//d
    }
}
```

# 7、codePointAt（）

- 获取字符的码点值

```java
public class StringApi {
    public static void main(String[] args) {
    
        String x1 = "abcdefgh1jilif";
        
        int i = x1.codePointAt(3);
        
        System.out.println(i);// d字符在编码表的代码值为100
    
    }
}
```

