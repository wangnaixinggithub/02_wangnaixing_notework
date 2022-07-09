

# 1、@RequestParam

> 下面展示如何获取请求参数

```java
    @GetMapping("/test")
    @ResponseBody
    public String testHandler1(
            @RequestParam(value = "username",defaultValue = "admin") String username,
            @RequestParam(value = "password",defaultValue = "123456") String password){
        //提供默认值 username=admin password=123456
        log.info("username=>>{}",username);
        log.info("password=>>{}",password); 
        return "hello";
    }

    @GetMapping("/test2")
    @ResponseBody
    public String testHandler2( String username,String password){
        //接收请求参数为username password的值
        log.info("username=>>{}",username);
        log.info("password=>>{}",password); 
        return "hello";
    }

 	@GetMapping("/test3")
    @ResponseBody
    public String testHandler3(User user){
        //请求参数和POJO属性要一一对应，接收到请求参数，统一封装的POJO中。
        log.info("username=>>{}",user.getUsername());
        log.info("password=>>{}",user.getPassword());
        return "hello";
    }

	@GetMapping("/test4")
    @ResponseBody
    public String testHandler4(Map<String,Object> map){
        //请求参数和POJO属性要一一对应，接收到请求参数，统一封装的POJO中。
        log.info("username=>>{}",map.get("username"));
        log.info("password=>>{}",map.get("password"));
        return "hello";
    }

}	
```

均能获取到请求参数username和password!

```c
[2022-05-08 11:06:17:363] [INFO] - com.wnx.springmvc.controller.UserController.testHandler(UserController.java:29) - username=>>admin
[2022-05-08 11:06:17:363] [INFO] - com.wnx.springmvc.controller.UserController.testHandler(UserController.java:30) - password=>>123456
```

# 2、@RequestHandler

> 获取请求的请求头信息，能拿到F12中展示的全部请求头信息。

```java
 @GetMapping("/test")                            
    @ResponseBody
    public String testHandler(
            @RequestHeader(name = "Accept")String accept,
            @RequestHeader(name = "Accept-Encoding")String acceptEncoding,
            @RequestHeader(name = "Cache-Control") String cacheControl,
            @RequestHeader(name = "Connection") String connection,
            @RequestHeader(name = "Host") String host,
            @RequestHeader(name = "Pragma") String pragma,
            @RequestHeader(name = "User-Agent") String userAgent

    ){
        log.info("Accept => {}",accept);
        log.info("Accept-Encoding => {}",acceptEncoding);
        log.info("Cache-Control => {}",cacheControl);
        log.info("Connection => {}",connection);
        log.info("Host => {}",host); 
        log.info("Pragma => {}",pragma);
        log.info("User-Agent => {}",userAgent);
        return "hello";
    }
    
```

![](002-SpringMVC-handler%E6%96%B9%E6%B3%95%E5%BD%A2%E5%8F%82%E7%9A%84%E4%BD%BF%E7%94%A8.assets/image-20220508112732502.png)

```c++
[2022-05-08 11:27:54:900] [INFO] - com.wnx.springmvc.controller.UserController.testHandler(UserController.java:33) - Accept => text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
[2022-05-08 11:27:54:901] [INFO] - com.wnx.springmvc.controller.UserController.testHandler(UserController.java:34) - Accept-Encoding => gzip, deflate, br
[2022-05-08 11:27:54:901] [INFO] - com.wnx.springmvc.controller.UserController.testHandler(UserController.java:35) - Cache-Control => no-cache
[2022-05-08 11:27:54:902] [INFO] - com.wnx.springmvc.controller.UserController.testHandler(UserController.java:36) - Connection => keep-alive
[2022-05-08 11:27:54:902] [INFO] - com.wnx.springmvc.controller.UserController.testHandler(UserController.java:37) - Host => localhost:8080
[2022-05-08 11:27:54:902] [INFO] - com.wnx.springmvc.controller.UserController.testHandler(UserController.java:38) - Pragma => no-cache
```

# 3、@CookieValue 

```java
@GetMapping("/test1")
@ResponseBody
public String testHandler(HttpServletResponse httpServletResponse){
    httpServletResponse.addCookie(new Cookie("username","wangnaixing"));
    httpServletResponse.addCookie(new Cookie("currentDate","2022-05-08"));
    return "hello";
}

@GetMapping("/test2")
@ResponseBody
public String testHandler2(
        @CookieValue(name = "username") String username,
        @CookieValue(name = "currentDate") String currentDate
                           ){
      log.info("username => {} ",username);
      log.info("currentDate => {} ",currentDate);

      return "hello";
}
```

- test1的请求，制作cookie，此时会给浏览器响应set-cookies字段。

![](002-SpringMVC-handler%E6%96%B9%E6%B3%95%E5%BD%A2%E5%8F%82%E7%9A%84%E4%BD%BF%E7%94%A8.assets/image-20220508114541281.png)

- test2发起请求，请求携带cookies

![](002-SpringMVC-handler%E6%96%B9%E6%B3%95%E5%BD%A2%E5%8F%82%E7%9A%84%E4%BD%BF%E7%94%A8.assets/image-20220508114619897.png)

- `testHandler2()`方法，控制台输出两个cookies值。

```c++
[2022-05-08 11:44:16:504] [INFO] - com.wnx.springmvc.controller.UserController.testHandler2(UserController.java:41) - currentDate => 2022-05-08 
[2022-05-08 11:44:19:408] [INFO] - com.wnx.springmvc.controller.UserController.testHandler2(UserController.java:40) - username => wangnaixing 
[2022-05-08 11:44:19:408] [INFO] - com.wnx.springmvc.controller.UserController.testHandler2(UserController.java:41) - currentDate => 2022-05-08 
```



