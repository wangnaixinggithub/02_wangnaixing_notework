# SpringBoot-@ControllerAdvice-@ExceptionHandler

> 标识有@ControllerAdvice的类就是一个异常处理类了，并通过@ExceptionHandler,指定当下处理的是哪一个异常。

- 我们知道，如果你的程序出现了异常，按照异常总是向上抛起，最后会传递到前端控制器。此时前端控制器会把异常交给标识有@ControllerAdvice的组件，来处理异常，比如我自己写的处理逻辑就是把错误信息写到错误页面中展示一下。

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

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/120-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-SpringBoot%E7%9A%84%E6%8F%90%E4%BE%9B@ControllerAdvice%E6%9D%A5%E8%BF%9B%E8%A1%8C%E5%85%A8%E5%B1%80%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86.assets/image-20220327165351261.png)

