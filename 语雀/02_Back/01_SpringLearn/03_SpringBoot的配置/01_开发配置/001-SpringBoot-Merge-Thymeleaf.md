# SpringBoot-Merge-Thymeleaf



## 1、`xmlns:th="http://www.thymeleaf.org">`

```yaml
# application.yaml
spring:   
  thymeleaf:
    prefix: classpath:/templates
    suffix: .html
    encoding: UTF-8
    mode: HTML
    template-resolver-order: 1
    cache: false
    check-template: true
    check-template-location: true
    enable-spring-el-compiler: false
    enabled: true
    servlet:
      content-type: text/html

```

```xml
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
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
    <tr th:each="item:${userList}">
        <td th:text="${item.getUsername()}" t></td>
        <td th:text="${item.getNickName()}"></td>
        <td th:text="${item.getSex()}"></td>
    </tr>
    </tbody>
</table>
</body>
</html>
```

## 2、JS-Get-Response-Model

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
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
    <tbody id="tbody">

    </tbody>
</table>
<script th:inline="javascript">
    let userList = [[${userList}]]
    let element = document.getElementById("tbody")
    let trHtml = ''
    userList.forEach(item=>{
        trHtml += `
        <tr>
               <td>${item.username}</td>
               <td>${item.nickName}</td>
               <td>${item.sex}</td>
        </tr>
        `
    })
   element.innerHTML = trHtml
</script>
</body>
</html>
```

## 3、As-Email-Template

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>邮件模板</title>
</head>
<body>
<p>
    Hello！欢迎<span th:text="${username}"/>
</p>
<p>
    您的职位信息为：<span th:text="${position}"></span>
</p>
<p>
    报到地址：<span th:text="${address}"></span>
</p>
<p>
    入职联系人:<span th:text="${contactMan}"></span>
</p>
<p>
    薪资待遇：<span th:text="${salary}"></span>
</p>
</body>
</html>
```

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @Autowired
    private TemplateEngine templateEngine;
    
    @GetMapping("/sendEmail")
    public void sendEmail(){
        Context context = new Context();
        context.setVariable("username","王乃醒");
        context.setVariable("position","实习生");
        context.setVariable("address","广东省广州市天河区华强路一号珠控国际中心803");
        context.setVariable("contactMan","洪磊");
        context.setVariable("salary","150/天");
        String email = templateEngine.process("email", context);
        //省略邮件发送API调用过程.....
        
    }
 }
```
