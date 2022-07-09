# JavaBean-equals-And-hashCode

```java
@AllArgsConstructor
@NoArgsConstructor
@Getter
@Setter
public class User implements Serializable {
    private String name;
}
```

# 1、equals

> 比引用，即堆区的内存块地址标识。

```java
@Test
public void test01() {
    User user = new User("王乃醒");
    User user2 = new User("王乃醒");
    User user3 = user2;
    System.out.println(user.equals(user2));   //false
    System.out.println(user3.equals(user2));    //true
    
}
```



# 2、hashCode

```java
    @Test
    public void test01() {
        User user = new User("王乃醒");
        System.out.println(user.hashCode());    //1160003871

    }

```

