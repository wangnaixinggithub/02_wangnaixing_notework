# SpringMvc-@RequestBody-@ResponseBody-`RequestEntity<String>`

# 1、@RequestBody

## 1.1 、获取负载

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>获取负载</title>
</head>
<body>
<form th:action="@{/testRequestBody}" method="post">
  用户名：<input type="text" name="username"><br>
  密码：<input type="password" name="password"><br>
  <input type="submit">
</form>
</body>
</html>
```

```java
    @RequestMapping("/testRequestBody")
    public String testRequestBody(@RequestBody String jsonStr){
        System.out.println("requestBody:"+jsonStr); //requestBody:username=767564319%40qq.com&password=123456
        return "success";
    }
```

## 1.2、负载解析为Model

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>负载解析为Model</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.26.1/axios.min.js"></script>
</head>
<body>
<div id="app">
    <a th:href="@{/testAjax}" @click="testAjax">testAjax</a><br>
</div>
<script>
    let vue = new Vue({
        el:"#app",
        methods:{
            testAjax:function (event){
                axios({
                    method:"POST",
                    url:event.target.href,
                    params:{
                        username:"wangnaixing",
                        password:"123456"
                    }
                }).then(res=>{
                    console.log(res.data)
                })
                event.preventDefault()
            }

        }
    })
</script>
</body>
</html>
```

```java
 @PostMapping("/testAjax")
    @ResponseBody
    public String testAjax(@RequestBody User user){
        System.out.println("username:"+user.getUsername()+",password:"+user.getPassword());
        return "hello,ajax";
    }
```

# 2、`RequestEntity<String>`

- RequestEntity相当于一次请求的请求报文，从请求报文中，我们可以得到请求头信息，请求体信息，请求方法，请求的URL.

```java
 @RequestMapping("/testRequestBody")
    public String testRequestBody(RequestEntity<String> requestEntity){
        System.out.println("请求体信息："+requestEntity.getBody());
        System.out.println("请求头信息" + requestEntity.getHeaders());
        System.out.println("请求方法"+requestEntity.getMethod());
        System.out.println("请求的URL"+requestEntity.getUrl());
        return "success";
    }
```

- 输出结果如下

```c#
请求体信息：username=767564319%40qq.com&password=123456
请求头信息[host:"localhost:8080", connection:"keep-alive", content-length:"43", cache-control:"max-age=0", sec-ch-ua:"" Not A;Brand";v="99", "Chromium";v="99", "Microsoft Edge";v="99"", sec-ch-ua-mobile:"?0", sec-ch-ua-platform:""Windows"", upgrade-insecure-requests:"1", origin:"http://localhost:8080", content-type:"application/x-www-form-urlencoded", user-agent:"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36 Edg/99.0.1150.39", accept:"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9", sec-fetch-site:"same-origin", sec-fetch-mode:"navigate", sec-fetch-user:"?1", sec-fetch-dest:"document", referer:"http://localhost:8080/32_aiguigu_springmvc_httpMessageConverter_war_exploded/", accept-encoding:"gzip, deflate, br", accept-language:"zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6"]
请求方法POST
请求的URLhttp://localhost:8080/32_aiguigu_springmvc_httpMessageConverter_war_exploded/testRequestBody
请求类型：java.lang.String
```

# 3、@ResponseBody

- 直接作用在handler方法上，可以把handler方法的返回值，写入响应报文的响应体中。返回给浏览器。

```java
    @GetMapping("/test")
    public String toSuccess(){
        return "success";
    }

    @GetMapping("/test2")
    @ResponseBody
    public String handler(){
        //这个时候就不是视图了，而是把返回值写入的响应报文的响应体中。！
        return "success";
    }

```

- 进行对象序列化成JSON,写入响应报文的响应体中，返回给浏览器。

```java
	@GetMapping("/test2")
    @ResponseBody
    public User handler(){
        //内部会把对象序列化成JSON字符串写入响应报文的响应体中！
        return new User("wangnaixing","123456"); //{"username":"wangnaixing","password":"123456"}
    }

```
