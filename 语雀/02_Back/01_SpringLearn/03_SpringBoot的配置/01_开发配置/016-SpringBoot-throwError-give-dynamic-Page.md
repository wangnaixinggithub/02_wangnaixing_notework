# SpringBoot-throwError-give-dynamic-Page

> 除了可以根据@ControllerAdvice来统一处理异常之外，还可以通过提供动态页面的方式来处理异常

- 在template模板文件夹下，新建error文件夹，创建4xx.html,创建500.xml,当出现异常时，不需要开发者做路径映射，SpringMVC会自动的找到异常。并且根据状态码跳转到对应的动态网页上。

# 1、4xx.html

```html

<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>处理状态码是400-499的异常</title>
</head>
<body>
<h1>系统出现了异常哦！</h1>
<table>
    <tr>
        <td>请求路径</td>
        <td th:text="${path}"></td>
    </tr>
    <tr>
        <td>请求错误时间</td>
        <td th:text="${timestamp}"></td>
    </tr>
    <tr>
        <td>请求状态码</td>
        <td th:text="${error}"></td>
    </tr>
</table>
</body>
</html>
```

# 2、5xx.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>处理状态码是500-599的异常</title>
</head>
<body>
<h1>系统出现了异常哦！</h1>
<table>
    <tr>
        <td>请求路径</td>
        <td th:text="${path}"></td>
    </tr>
    <tr>
        <td>请求错误时间</td>
        <td th:text="${timestamp}"></td>
    </tr>
    <tr>
        <td>请求状态码</td>
        <td th:text="${status}"></td>
    </tr>
    <tr>
        <td>错误信息</td>
        <td th:text="${error}"></td>
    </tr>
</table>
</body>
</html>
```

