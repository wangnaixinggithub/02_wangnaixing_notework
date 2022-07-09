# SpringMVC-全局异常处理

## 1、GlobalExceptionHandler,处理器Exception异常。

```java
package com.wnx.springmvc.handler;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.ModelAndView;

@ControllerAdvice
public class GlobalExceptionHandler {


    @ExceptionHandler(Exception.class)
    public ModelAndView handleException(Exception e){
        ModelAndView mv = new ModelAndView("error");
        mv.addObject("errorMessage",e.getMessage());
        return mv;

    }
}

```

## 2、模拟程序出现了异常。

```java
package com.wnx.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class ExceptionController {

    @GetMapping("/exception")
    public String toUploadPage(){
        int i  = 10/0;
        return "hello";
    }

}

```

## 3、该异常会抛给GlobalExceptionHandler处理。

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>错误信息展示页面</title>
</head>
<body>
<h2 th:text="${errorMessage}">出现了错误信息</h2>
</body>
</html>
```

