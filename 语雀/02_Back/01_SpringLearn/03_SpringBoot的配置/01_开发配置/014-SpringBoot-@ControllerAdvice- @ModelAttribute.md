# SpringBoot-@ControllerAdvice- @ModelAttribute

- 在`@ControllerAdivce`组件上定义方法，返回全局变量数据。并在方法上使用`@ModelAttribute`注解标识.

- 可以在里头指定全局变量的`key,`    默认不指定时，是返回时的变量名。

```java
@ControllerAdvice
public class ControllerExceptionHandler {
    @ModelAttribute("globalData")
    public Map<String, Object> globalData(){
        Map<String, Object> map = new HashMap<>();
        map.put("id",1);
        map.put("username","root");
        map.put("nickName","王乃醒");
        map.put("age",20);
        return map;
    }
}
```

- 我分别在AccountController,UserController中通过在方法中注入Model变量。调用asMap()方法，便可以得到一个以key=globalData,value=Map集合的一个Map

```java
//AccountController
@Slf4j
@RestController
@RequestMapping("/account")
public class AccountController {
    @ResponseBody
    @GetMapping("/globalData")
    public String error(Model model){
        log.info("globalData->{}",model.asMap());
        return "SUCCESS";
    }
}

//UserController
@Slf4j
@Controller
@RequestMapping("/user")
public class UserController {
    @ResponseBody
    @GetMapping("/globalData")
    public String error(Model model){
        log.info("globalData->{}",model.asMap());
        return "SUCCESS";
    }
}
```

```java
2022-03-27 17:03:16.451  INFO 15876 --- [nio-8080-exec-1] com.wnx.boot.controller.UserController   : globalData->{globalData={nickName=王乃醒, id=1, age=20, username=root}}
2022-03-27 17:03:23.304  INFO 15876 --- [nio-8080-exec-5] c.wnx.boot.controller.AccountController  : globalData->{globalData={nickName=王乃醒, id=1, age=20, username=root}}
```

- 如果我全局变脸如果存的不是Map而是User类。
- 我看看这个方法，会不会把User类封装成key为golobalData,值为User类的一个Map返回。

![image-20220327171055299](121-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-SpringBoot%E6%94%AF%E6%8C%81%E5%9C%A8%E6%A0%87%E8%AF%86%E4%BA%86@ControllerAdvice%E7%9A%84%E7%BB%84%E4%BB%B6%E4%B8%8A%E5%AE%9A%E4%B9%89%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F.assets/image-20220327171055299.png)

```java
@ControllerAdvice
public class ControllerExceptionHandler {
    @ModelAttribute("globalData")
    public User globalData(){
        return User.builder().id(1).username("root").nickName("王乃醒").sex("男").build();
    }
}
```

```java
//SUCCESS 可行！！！
@Slf4j
@Controller
@RequestMapping("/user")
public class UserController {
    @ResponseBody
    @GetMapping("/globalData")
    public String error(Model model){
        log.info("{}",model.getAttribute("globalData"));
        return "SUCCESS";
    }
}
```

```coffeescript
2022-03-27 17:14:14.418  INFO 31068 --- [nio-8080-exec-2] c.wnx.boot.controller.AccountController  : globalData->{globalData=User(id=1, username=root, nickName=王乃醒, sex=男)}
```

