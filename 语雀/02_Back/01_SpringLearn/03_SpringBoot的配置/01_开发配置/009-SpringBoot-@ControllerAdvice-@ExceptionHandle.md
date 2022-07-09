# SpringBoot-@ControllerAdvice-@ExceptionHandler

```java
@ControllerAdvice
public class ControllerExceptionHandler {
    @ExceptionHandler(Exception.class)
    public ModelAndView handleException(Exception exception){
        ModelAndView modelAndView = new ModelAndView("error");
        modelAndView.addObject("errorMsg",exception.getMessage());
        return modelAndView;
    }
}
```

```xml
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>错误页面</title>
</head>
<body>
<h1>不好意思，系统出现了错误恩</h1>
<p th:text="${errorMsg}"></p>
</body>
</html>
```

![image-20220327165351261](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/120-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-SpringBoot%E7%9A%84%E6%8F%90%E4%BE%9B@ControllerAdvice%E6%9D%A5%E8%BF%9B%E8%A1%8C%E5%85%A8%E5%B1%80%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86.assets/image-20220327165351261.png)

