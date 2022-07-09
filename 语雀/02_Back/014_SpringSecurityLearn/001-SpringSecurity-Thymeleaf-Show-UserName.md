# SpringSecurity-Thymeleaf-Show-UserName

> 在使用`SpringSecurity`的时候，假如你的前端用的是模板引擎，比如`thymeleaf` 如何获取当前登录用户的用户名呢？

## 1、Need Config

> `thymeleaf` 主动的去整合了`SpringSecurity`.

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-springsecurity5</artifactId>
        </dependency>
```

# 2、Use It

```html
     <a href="javascript:;" th:text="${#authentication.name}"></a>
```
