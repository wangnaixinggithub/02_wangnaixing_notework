# Java-Scanner-Class-#nextXxx()

- Scanner类，可以用来获取用户在控制台的输入。

# 1、#nextInt()

- 读取用户输入，转为整数

```java
public class Test {

    public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
        int i = scanner.nextInt();
        System.out.println(i);
    }
}
```

# 2、#next()

- 读取用户输入，转为字符串。

```java
public class Test {
    public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
        String str = scanner.next();
        System.out.println(str);
    }
}
```

# 3、#nextDouble()

- 读取用户输入，转为双精度类型。

```java
public class Test {
    public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
        Double nextDouble = scanner.nextDouble();
        System.out.println(nextDouble);
    }
}
```

# 4、#nextFloat()

- 读取用户输入，转为单精度类型。

```java
public class Test {
    public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
        Float nextFloat = scanner.nextFloat();
        System.out.println(nextFloat);
    }
}
```

# 5、#nextBoolean()

- 读取用户输入，转为布尔类型。

```java
public class Test {
    public static void main(String[] args) {
           Scanner scanner = new Scanner(System.in);
            Boolean nextBoolean = scanner.nextBoolean();
            System.out.println(nextBoolean);
        }
}
```

# 6、#nextLong()

- 读取用户输入，转为长整型。

```java
public class Test {
    public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
        Long nextLong = scanner.nextLong();
        System.out.println(nextLong);
    }
}
```

# 7、#nextBigDecimal()

- 读取用户输入，转为BigDecimal类型。

```java
public class Test {
    public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
        BigDecimal nextBigDecimal = scanner.nextBigDecimal();
        System.out.println(nextBigDecimal);
    }
}
```

# 8、#nextBigInteger()

- 读取用户输入，转为BigInteger类型。

```java
public class Test {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        BigInteger bigInteger = scanner.nextBigInteger();
    }
}
```

