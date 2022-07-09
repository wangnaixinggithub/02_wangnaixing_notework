# SpringMVC-@RequestMappering的使用

# 1、处理URL请求

```java
@Controller
public class MyController  {

    @RequestMapping("/hello")
    public ModelAndView handler(){
        System.out.println("hh11h1");
        ModelAndView modelAndView = new ModelAndView("hello");
        modelAndView.addObject("name","王乃醒");
        return modelAndView;
    }
}

```

# 2、请求范围窄化

```java
@Controller
@RequestMapping("/user")
public class MyController  {

    @RequestMapping("/hello")
    public ModelAndView handler(){
        System.out.println("hh11h1");
        ModelAndView modelAndView = new ModelAndView("hello");
        modelAndView.addObject("name","王乃醒");
        return modelAndView;
    }
}

```

# 3、多个URL执行同一方法

```java
@Controller
public class MyController  {

    @RequestMapping(value = {"/hello","hello2","hello3"})
    public ModelAndView handler(){
        System.out.println("hh11h1");
        ModelAndView modelAndView = new ModelAndView("hello");
        modelAndView.addObject("name","王乃醒");
        return modelAndView;
    }
}
```

# 4、支持Ant路径风格的URL

```java
@Controller
public class HelloController {
    //包一层的
    @RequestMapping("/ant/*/abc")
    public String helloWorld(){
        System.out.println("包一层的");
        return "success";
    }
    //包许多层的
    @RequestMapping("/ant/**/abc")
    public String helloWorld2(){
        System.out.println("包许多层的");
        return "success";
    }
    //两个字符随意的
    @RequestMapping("/ant/abc??")
    public String helloWorld3(){
        System.out.println("两个字符随意的");
        return "success";
    }
}
```

# 5、路径占位符@PathVariable

```java
@Controller
public class HelloController {

    @RequestMapping("/user/{id}")
    public String helloWorld(@PathVariable("id")Integer id){
        return "success";
    }
   
}
```

