# SpringMVC-@ExceptionHandler-异常处理

# 1、 @ExceptionHandler

```java
    @GetMapping
    public String update() {
        int i = 10/0;
        System.out.println("Handler方法执行了哦..............");
        return "success";
    }

```

- 作用在当前接口

```java
    @ExceptionHandler(value = {ArithmeticException.class})
    public ModelAndView error(Exception error){
        System.out.println("ArithmeticException...");
        ModelAndView mv = new ModelAndView("error");
        mv.addObject("error",error);
        return mv;
    }
```

- 可以配置多个异常处理器

```java
  @ExceptionHandler(value = {ArithmeticException.class}) //他会被执行！
    public ModelAndView error(Exception error){
        System.out.println("ArithmeticException...");
        ModelAndView mv = new ModelAndView("error");
        mv.addObject("error",error);
        return mv;
    }
    @ExceptionHandler(value = {RuntimeException.class})
    public ModelAndView error2(Exception error){  //支持配置多个
        System.out.println("RuntimeException...");
        ModelAndView mv = new ModelAndView("error");
        mv.addObject("error",error);
        return mv;
    }
```

# 2、@ControllerAdvice

```java
@ControllerAdvice
public class ExceptionAdviceHandler {

    @ExceptionHandler(value = {ArithmeticException.class})
    public ModelAndView error(Exception error){
        System.out.println("ArithmeticException...");
        ModelAndView mv = new ModelAndView("error");
        mv.addObject("error",error);
        return mv;
    }
    @ExceptionHandler(value = {RuntimeException.class})
    public ModelAndView error2(Exception error){
        System.out.println("RuntimeException...");
        ModelAndView mv = new ModelAndView("error");
        mv.addObject("error",error);
        return mv;
    }
}
```

# 3、@ResponseStatus

- 自定义异常，可以定制响应内容。

```java
@ResponseStatus(value = HttpStatus.FORBIDDEN,reason = "用户名称和密码不匹配")
public class UsernameNotMatchPasswordException extends RuntimeException {
    public UsernameNotMatchPasswordException(String message) {
        super(message);
    }
}
```

```java
@Controller
@RequestMapping("/user")
public class HelloController {
    @GetMapping("{id}")
    public String update(@PathVariable("id")Integer id) {
        if (id > 3){
            throw new UsernameNotMatchPasswordException("发生异常");
       }
        return "success";
    }
}
```

- 抛出异常后，网页提示信息如下

![](012-SpringMvc-@ExceptionHandler-And-@ControllerAdvice.assets/image-20220320145840409.png)

