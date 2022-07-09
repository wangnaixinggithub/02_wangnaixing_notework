# SpringBoot解决跨域

![](018-SpringBoot-CrossOrigin.assets/image-20220328210336026.png)

```java
//localhost:8081/的接口
@RestController
public class HelloController {
    @GetMapping("/hello1")
    public String hello(){
        return "hello1";
    }
    @PostMapping("/hello2")
    public String hello2(){
        return "hello2";
    }
}
```

```html
<!--localhost:8080/的页面客户端 -->
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>同源策略</title>
</head>
<body>
<div id="app">

</div>
<button onclick="helloGet()">GET_click</button>
<button onclick="helloPost()">POST_click</button>
<script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
<script>
    
    function helloGet() {
       $.get(`http://localhost:8081/hello1`,function (msg) {
           document.getElementById("app").innerHTML = msg
       })
    }

    function helloPost(){
        $.post(`http://localhost:8081/hello2`,function (msg) {
            document.getElementById("app").innerHTML = msg
        })

    }
</script>
</body>
</html>
```

# 1、@CrossOrigin

> 标识这个接口，可以让http://localhost:8080的主机访问。

```java
@RestController
public class HelloController {
    @GetMapping("/hello1")
    @CrossOrigin("http://localhost:8080")
    public String hello(){
        return "hello1";
    }
    @PostMapping("/hello2")
    @CrossOrigin(value = "http://localhost:8080")
    public String hello2(){
        return "hello2";
    }
}

```

![](018-SpringBoot-CrossOrigin.assets/image-20220328211302854.png)

# 2、addCorsMappings

> 全局配置

```java
@Configuration
public class WebmvcConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {      registry.addMapping("/**").allowedOrigins("http://localhost:8080").allowedMethods("*").allowedHeaders("*");
    }
}
```

# 3、FilterRegistrationBean

```java
@Bean
public FilterRegistrationBean corsFilter() {
    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    CorsConfiguration config = new CorsConfiguration();
    config.setAllowCredentials(true);
    config.addAllowedOrigin("*");
    source.registerCorsConfiguration("/**", config);
    FilterRegistrationBean bean = new FilterRegistrationBean(new CorsFilter(source));
    bean.setOrder(0);
    return bean;
}
```

