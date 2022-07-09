# SpringBoot-Use-GlobalExceptionHandler

# 1、Boot Default Exception Handler

- 假定系统接口异常

```java
@Api(tags = "ExceptionController",description = "异常机制测试")
@RequestMapping("/error")
@RestController
public class ExceptionController {

    @GetMapping("/getError")
    public CommonResult getError(){

        throw new RuntimeException("YOUR SYSTEM ERROR!");

    }
}

```

- 当你的系统出现异常的时候，异常处理机制会判断，请求头中的`accept` 的内容，

```properties
如果 accept = text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9 => 就返回错误页面。

如果 accept = */* => 就返回错误JSON 具体如下两张图所示
```

- 给用户呈现的错误页面

![image-20220107184838609](https://s2.loli.net/2022/01/07/3gP9aFwJqCtQdzv.png)

- 给用户呈现的错误JSON

![image-20220107184904917](https://s2.loli.net/2022/01/07/aSXdcEx96zIToKu.png)

- 机制底层

> `BasicErrorController` 基本的异常控制器，他的errorHtml error 方法，请求路径一样，区别在于produces = text/html就给页面，不然就给JSON。

```java
@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {

	private final ErrorProperties errorProperties;

	public static final String TEXT_HTML_VALUE = "text/html";	

	@RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
	public ModelAndView errorHtml(HttpServletRequest request,
			HttpServletResponse response) {
		HttpStatus status = getStatus(request);
		Map<String, Object> model = Collections.unmodifiableMap(getErrorAttributes(
				request, isIncludeStackTrace(request, MediaType.TEXT_HTML)));
		response.setStatus(status.value());
		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
		return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);
	}

	@RequestMapping
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
		Map<String, Object> body = getErrorAttributes(request,
				isIncludeStackTrace(request, MediaType.ALL));
		HttpStatus status = getStatus(request);
		return new ResponseEntity<>(body, status);
	}
}
```

# 2、GlobalExceptionHandler（HTML）

```java
package com.wnx.mall.tiny.config;


import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseStatus;

import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public ModelAndView defaultErrorHandler(HttpServletRequest request, Exception e) {
        ModelAndView mav = new ModelAndView("500.html"); // 设置跳转路径
        
        mav.addObject("exception", e); // 将异常对象传递过去
       
        mav.addObject("url", request.getRequestURL()); // 获得请求的路径
        
        return mav;
    }



```

- 500.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>500</title>
</head>
<body>
<body>
<p th:text="${url}"></p>
<p th:text="${exception.message}"></p>
</body>
</body>
</html>
```

![image-20220107202556644](https://s2.loli.net/2022/01/07/m8iXVsadrfOAEWl.png)

# 3、GlobalExceptionHandler（JSON）

```java
package com.wnx.mall.tiny.config;


import com.wnx.mall.tiny.common.api.CommonResult;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import javax.servlet.http.HttpServletRequest;

@RestControllerAdvice
public class GlobalExceptionHandler {


    @ExceptionHandler(Exception.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public CommonResult defaultErrorHandler(HttpServletRequest request, Exception e) {
        
        return CommonResult.failed(e.getMessage());

    }


}
```

