

# 1、ModelAndView

```java
@Controller
public class MyController  {

    @RequestMapping("/hello")
    public ModelAndView handler(){
        System.out.println("hh11h1");
        ModelAndView modelAndView = new ModelAndView("hello");
        modelAndView.addObject("name","王乃醒"); //数据模型+视图
        return modelAndView;
    }
}

```

# 2、String

- 返回逻辑视图

```java
@Controller
public class MyController  {

    @RequestMapping("/hello")
    public String handler(Model model) throws ServletException, IOException {
        model.addAttribute("name","王乃醒");
        //去找视图
        return "hello";

    }
}

```

- 转发

```java
@Controller
public class MyController  {

    @RequestMapping("/hello")
    public String handler(Model model) throws ServletException, IOException {
        model.addAttribute("name","王乃醒");
        //转发
        return "forward:hello";

    }
}

```

- 重定向

```java
@Controller
public class MyController  {

    @RequestMapping("/hello")
    public String handler(Model model) throws ServletException, IOException {
        model.addAttribute("name","王乃醒");
        //重定向
        return "redirect:hello";

    }
}
```

- 返回字符串

```java
@Controller
public class MyController  {
    @RequestMapping("/hello")
    @ResponseBody
    public String handler() {
        return "Wangaixing is coding Boy";
    }
}

```

> 如果是中文字符串返回的话浏览器先出现乱码，可以用produces = "text/html;charset=utf-8" 规定解析的字符集为UTF-8。

```java
@Controller
public class MyController  {

    @RequestMapping(value = "/hello",produces = "text/html;charset=utf-8")
    @ResponseBody
    public String handler() {
        return "王乃醒";
    }
}

```

