# Override-Object-equal()-hashCode()

> 默认情况下，equal()方法是比较引用，我们通常是想比较对象里头属性值都相等，就可以认为他相等了。并不关心他是不是同一内存块，这时就必须重写equal() And hashCode()。如下提供三种重写方式。

# 1、Objects#equals

```java
@AllArgsConstructor
@NoArgsConstructor
@Getter
@Setter
public class User implements Serializable {
    private String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        
        if (o == null || getClass() != o.getClass()) return false;
        
        User user = (User) o;
        return Objects.equals(name, user.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}
```

```java
    @Test
    public void test_Override_equals() {
        User u1 = new User("王乃醒");
        User u2 = new User("王乃醒");

        Assertions.assertTrue(u1.equals(u2));
        Assertions.assertTrue(u1.hashCode() == u2.hashCode());
    }
```

# 2、Apache Commons Lang 

```java
@AllArgsConstructor
@NoArgsConstructor
@Getter
@Setter
public class User implements Serializable {
    private String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return new EqualsBuilder().append(name,user.user.getName()).isEquals();
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}

```

# 3、Guava

```java
@AllArgsConstructor
@NoArgsConstructor
@Getter
@Setter
public class User implements Serializable {
    private String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return com.google.common.base.Objects.equal(name,user.getName());
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}
```

