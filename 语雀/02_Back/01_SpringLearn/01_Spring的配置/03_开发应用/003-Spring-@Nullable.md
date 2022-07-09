# Spring-@Nullable注解

> 有这个注解的地方，代表形参参数值、方法返回值、属性值-可以  为空。

## 1、属性

```java
public class User implements Serializable {
    private Integer id;
    //1.可以作用在属性上，表示name属性可以为空
    @Nullable
    private String name;
    //2.可以作用在方法上，表示方法返回值可以为空
    @Nullable
    public String getName() {
        return name;
    }

    public Integer getId() {
        return id;
    }
}

```

## 2、方法

```java
public class User implements Serializable {
    private Integer id;
    //1.可以作用在属性上，表示name属性可以为空
    @Nullable
    private String name;
    //2.可以作用在方法上，表示方法返回值可以为空
    @Nullable
    public String getName() {
        return name;
    }

    public Integer getId() {
        return id;
    }
}

```

## 3.形参

```java
public class NullAbleTest {
    //3.可以作用在方法形参上，表示方法这个参数可以为空
    public void method(@Nullable String username){
        System.out.println("哈哈！！");
    }

}
```
