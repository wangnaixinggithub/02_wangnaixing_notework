# JavaClass-Scanner-#hasNext()

- 如果用户在控制台有输入，就为`true`.

```java
public class Test {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        boolean hasNext = scanner.hasNext();
        System.out.println(hasNext);    // always is True!
    }
}
```

