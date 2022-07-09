# Scanner-#next()-#nextLine()

> Scanner类中，next()方法和nextLine()方法的区别。

- 共同点：遇到空格，停止获取输入。
- 不同点：nextLine()接收空格，Tab键组成的所有字符，而next()只能接收单个不是空格，不是Tab键的字符。

# 1、nextLine()

- 可以接收空格，或者Tab键输入（等价于4个空格）

```java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String next = scanner.nextLine();
        System.out.println(StringUtils.isWhitespace(next));  //true
    }
}
```

- 可以接收多个字符组成的字符串。

```java
public class Main {
    public static void main(String[] args) {
        //可接受数字字符串、空格组成的字符串组合。
        Scanner scanner = new Scanner(System.in);
        String next = scanner.nextLine();
        System.out.println(next); //10 20 30  
    }
}
```

# 2、next()

- 只能接收单个字符，并且字符不能是空格 Tab。

```java
public class Main {
    public static void main(String[] args) {
        //输入Tab 再输入数字4
        Scanner scanner = new Scanner(System.in);
        String next = scanner.next();
        System.out.println(next); //4
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        //输入空格 再输入数字4
        Scanner scanner = new Scanner(System.in);
        String next = scanner.next();
        System.out.println(next); //4
    }
}
```

