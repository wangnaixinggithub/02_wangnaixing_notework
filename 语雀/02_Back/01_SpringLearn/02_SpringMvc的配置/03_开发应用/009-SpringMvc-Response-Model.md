# SpringMVC-Response-Model

# 1、ModelAndView

```java
@Controller
@RequestMapping("/user")
public class HelloController {
    @GetMapping
    public ModelAndView createUser(){
        ModelAndView modelAndView = new ModelAndView("success");
        modelAndView.addObject("user",
                User.builder()
                .username("王乃醒")
                .id(1)
                .age(18)
                .email("827376239@qq.com")
                .password("root").build());

        return modelAndView;
    }
}
```

# 2、Model,ModelMap,Map

```java
@Controller
@RequestMapping("/user")
public class HelloController {
    @GetMapping
    public String createUser(Map<String,Object> map){
        map.put("user",
                User.builder()
                .username("王乃醒")
                .id(1)
                .age(18)
                .email("827376239@qq.com")
                .password("root").build());
        return "success";
    }
}
```

```java
@Controller
@RequestMapping("/user")
public class HelloController {
    @GetMapping
    public String createUser(ModelMap modelMap){
        modelMap.put("user",
                User.builder()
                .username("王乃醒")
                .id(1)
                .age(18)
                .email("827376239@qq.com")
                .password("root").build());
        return "success";
    }
}
```

```java
@Controller
@RequestMapping("/user")
public class HelloController {
    @GetMapping
    public String createUser(Model model){
        model.addAttribute("user",
                User.builder()
                .username("王乃醒")
                .id(1)
                .age(18)
                .email("827376239@qq.com")
                .password("root").build());

        return "success";
    }
}
```

# 3、@SessionAttributes

> 使用了@SessionAttributes,会把值存在Session中。

```java
@Controller
@RequestMapping("/user")
@SessionAttributes("user")
public class HelloController {
    @GetMapping
    public String createUser(Model model){
        model.addAttribute("user",
                User.builder()
                .username("王乃醒")
                .id(1)
                .age(18)
                .email("827376239@qq.com")
                .password("root").build());
        return "success";
    }
}
```

# 4、@ModelAttribute

> 方法入参时，使用@ModelAttribute，会让请求入参先从请求域中获取一遍。再做请求参数到对象数组值的映射。

```java
@Controller
@RequestMapping("/user")
public class HelloController {

    @ModelAttribute
    @GetMapping
    public String findById(Model model){
        model.addAttribute("user",
                User.builder()
                .username("王乃醒")
                .id(1)
                .age(18)
                .password("root")
                .email("827376239@qq.com").build());
        return "success";
    }
    @PostMapping
    public String update(@ModelAttribute("user")User user){
        System.out.println("正在修改的用户->"+user);
        return "success";
    }
}
```

# 5、Servlet API

> 往Request域中存数据

```java
@Controller
public class RequestMappingController {
    @GetMapping("/test")
    @ResponseBody
    public String testHandler4(HttpServletRequest httpServletRequest){
         httpServletRequest.setAttribute("key","王乃醒是大帅哥！！");
        return "王乃醒是帅哥";
    }
}
```

> 往Session域中存数据

```java
@Controller
public class RequestMappingController {

    @GetMapping("/test")
    @ResponseBody
    public String testHandler4(HttpSession httpSession){
        httpSession.setAttribute("key","王乃醒是大帅哥！！");
        return "王乃醒是帅哥4";
    }

}
```

> 往Application域中存数据

```java
@Controller
public class RequestMappingController {
    @GetMapping("/test")
    @ResponseBody
    public String testHandler4(HttpSession httpSession){
        ServletContext servletContext = httpSession.getServletContext();
        servletContext.setAttribute("key","王乃醒是大帅哥！！");
        return "王乃醒是帅哥";
    }
}
```



