# SpringBoot-@JsonFormat

- 我们都知道，在Spring中使用`@ResponseBody`注解可以将方法返回的对象序列化成JSON，比如：

```java
@RequestMapping("getuser")
@ResponseBody
public User getUser() {
    User user = new User();
    user.setUserName("mrbird");
    user.setBirthday(new Date());
    return user;
}
```

```java
public class User implements Serializable {
    private static final long serialVersionUID = 6222176558369919436L;
    
    private String userName;
    private int age;
    private String password;
    private Date birthday;
    ...
}
```

- 访问`getuser`页面输出：

```
{"userName":"mrbird","age":0,"password":null,"birthday":1522634892365}
```

可看到时间默认以时间戳的形式输出，如果想要改变这个默认行为，我们可以注解

# 1、@JsonFormat

```java
public class User implements Serializable {
    private static final long serialVersionUID = 6222176558369919436L;
    
    private String userName;
    private int age;
    private String password;
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone = "GMT+8")
    private Date birthday;
    ...
}
```

- 再次访问`getuser`，页面输出：

```java
{"userName":"mrbird","age":0,"password":null,"birthday":"2018-04-02 10:14:24"}
```

