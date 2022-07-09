# SpringBoot-Megre-Freemarker

# 1、.ftlh

```yaml
spring:
  freemarker:
    cache: false
    content-type: text/html
    template-loader-path: /templates
    charset: UTF-8
    suffix: .ftlh
    allow-session-override: false
    allow-request-override: false
    check-template-location: true
    expose-spring-macro-helpers: true
    expose-request-attributes: false
    expose-session-attributes: false
    prefer-file-system-access: false
    enabled: true
```

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @GetMapping("/findAll")
    public ModelAndView findAllUser(){
        List<User> userList = new ArrayList<>();
        userList.add(User.builder().id(1).username("wnx1").nickName("王乃醒1").sex("男").build());
        userList.add(User.builder().id(2).username("wnx2").nickName("王乃醒2").sex("男").build());
        userList.add(User.builder().id(3).username("wnx3").nickName("王乃醒3").sex("男").build());
        ModelAndView modelAndView = new ModelAndView("userList");
        modelAndView.addObject("userList",userList);
        return modelAndView;
    }  
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户列表</title>
</head>
<body>
<table border="1">
    <thead>
    <tr>
        <td>用户名</td>
        <td>姓名</td>
        <td>性别</td>
    </tr>
    </thead>
    <tbody>
    <#list userList as item>
        <tr>
            <td>${item.getUsername()}</td>
            <td>${item.getNickName()}</td>
            <td>${item.getSex()}</td>
        </tr>
    </#list>
    </tbody>
</table>
</body>
</html>
```

