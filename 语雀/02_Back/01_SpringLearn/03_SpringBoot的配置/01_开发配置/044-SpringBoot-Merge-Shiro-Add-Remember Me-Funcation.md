# SpringBoot-Merge-Shiro-Add-Remember Me-Funcation

当用户成功登录后，关闭浏览器然后再打开浏览器访问http://localhost:8080/index,页面会跳转登录页，之前的登录因为浏览器的关闭已经失效了。

Shiro为我们提供了Remember Me功能，用户的登录状态不会因为浏览器的关闭而失效，知道Cookies过期。

# 1、Need Config

```xml
        <!--pom.xml-->
		<parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.1.3.RELEASE</version>
            <relativePath/>
        </parent>

  		<dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring</artifactId>
            <version>1.6</version>
        </dependency>

```

```yaml
# application.yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mallbefore?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
  thymeleaf:
    cache: false

mybatis:
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**/mapper/*.xml

logging:
  level:
    root: info
    com.wnx.mall: debug

```



# 2、Add Bean SimpleCookie

> 配置记住我cookies 并注入到记住我管理器，再把记住我管理器注入到安全管理器
>
>  map.put("/**", "user") 启用user拦截器，意思为用户认证通过或者配置了Remember Me 记住用户登录状态后可以访问。

```java
@Configuration
public class ShiroConfig {

   
    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(SecurityManager securityManager) {

        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();

 
        shiroFilterFactoryBean.setSecurityManager(securityManager);
        shiroFilterFactoryBean.setLoginUrl("/login");
        shiroFilterFactoryBean.setSuccessUrl("/index");
        shiroFilterFactoryBean.setUnauthorizedUrl("/403");

        LinkedHashMap<String, String> map = new LinkedHashMap<>();
        map.put("/css/**", "anon");
        map.put("/js/**", "anon");
        map.put("/fonts/**", "anon");
        map.put("/img/**", "anon");
        map.put("/druid/**", "anon");
        map.put("/logout", "logout");
        map.put("/", "anon");
        
        //map.put("/**", "user") 启用user拦截器，意思为用户认证通过或者配置了Remember Me 记住用户登录状态后可以访问
        map.put("/**", "user");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(map);

        return shiroFilterFactoryBean;
    }
    
    @Bean
    public ShiroRealm shiroRealm() {
        return new ShiroRealm();
    }



    @Bean
    public SecurityManager securityManager(CookieRememberMeManager rememberMeManager,ShiroRealm shiroRealm){
        DefaultWebSecurityManager securityManager =  new DefaultWebSecurityManager();
        
        securityManager.setRememberMeManager(rememberMeManager);
        securityManager.setRealm(shiroRealm);
      
        return (SecurityManager) securityManager;
    }

	//cookie对象
    @Bean
    public SimpleCookie rememberMeCookie() {
        //1.设置cookie名称，对应login.html页面的<input type="checkbox" name="rememberMe"/>
        SimpleCookie cookie = new SimpleCookie("rememberMe");
        
        //2.设置cookie的过期时间，单位为秒，这里为一天
        cookie.setMaxAge(86400);
        
        return cookie;
    }
    
	//cookie管理对象
    @Bean
    public CookieRememberMeManager rememberMeManager(SimpleCookie rememberMeCookie) {
        CookieRememberMeManager cookieRememberMeManager = new CookieRememberMeManager();
        cookieRememberMeManager.setCookie(rememberMeCookie);
        cookieRememberMeManager.setCipherKey(Base64.decode("4AvVhmFLUs0KTA3Kprsdag=="));  // rememberMe cookie加密的密钥
      
        return cookieRememberMeManager;
    }

}
```

```java
public class ShiroRealm extends AuthorizingRealm {

    @Resource
    private UmsAdminService adminService;

   
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

  
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
  
        String userName = (String) token.getPrincipal();
        String password = new String((char[]) token.getCredentials());
        UmsAdmin user = adminService.findByUsername(userName);

        if (user == null) {
            throw new UnknownAccountException("用户名或密码错误！");
        }

        if (!(user.getPassword().equals(password))){
            System.out.println(1111);
            throw new IncorrectCredentialsException("用户名或密码错误！");
        }
        
        if (user.getStatus() != 1) {
            throw new LockedAccountException("账号已被锁定,请联系管理员！");
        }
        
        SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(user, password, getName());
        
        return info;

    }
}

```

# 3、Your Business

- LoginController

> 构造UsernamePasswordToken时，把rememberMe状态注入！

```java


@Controller
public class LoginController {
    /**
     * 进入登录页
     * @return
     */
    @GetMapping("/login")
    public String login() {
        return "login";
    }

    @RequestMapping("/")
    public String redirectIndex() {
        return "redirect:/index";
    }

    /**
     * 进入首页
     * @param model
     * @return
     */
    @RequestMapping({"/","/index"})
    public String index(Model model) {
        
        UmsAdmin user = (UmsAdmin) SecurityUtils.getSubject().getPrincipal();
        
        model.addAttribute("user", user);
        
        return "index";
    }


    @ResponseBody
    @PostMapping("/login")
    public CommonResult login(String username, String password,Boolean rememberMe) {
       
        password = MD5Utils.encrypt(username, password);
       
        UsernamePasswordToken token = new UsernamePasswordToken(username, password,rememberMe);
       
        Subject subject = SecurityUtils.getSubject();

        try {
            
            subject.login(token);
          
            return CommonResult.success("");
            
        } catch (UnknownAccountException e) {
            
            return CommonResult.failed(e.getMessage());
            
        } catch (IncorrectCredentialsException e) {
            
            return CommonResult.failed(e.getMessage());
            
        } catch (LockedAccountException e) {
            
            return CommonResult.failed(e.getMessage());
            
        } catch (AuthenticationException e) {
            
            return CommonResult.failed("认证失败！");
        }
    }



}

```

- login.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
    <link rel="stylesheet" th:href="@{/css/login.css}" type="text/css">
    <script th:src="@{/js/jquery-3.3.1.js}"></script>
</head>
<body>
<div class="login-page">
    <div class="form">
        <input type="text" placeholder="用户名" name="username" required="required"/>
        <input type="password" placeholder="密码" name="password" required="required"/>
        <p><input type="checkbox" name="rememberMe" />记住我</p>
        <button onclick="login()">登录</button>
    </div>
</div>
</body>
<script th:inline="javascript">

        function login() {
            var username = $("input[name='username']").val();
            var password = $("input[name='password']").val();
            var rememberMe = $("input[name='rememberMe']").is(':checked');
            
            $.ajax({
                type: "post",
                url:  "http://localhost:8080/login",
                data: {"username": username,"password": password,"rememberMe": rememberMe},
                dataType: "json",
                success: function (r) {
                    if (r.code === 200) {
                        location.href = 'http://localhost:8080/index';
                    } else {
                        alert(r.message);
                    }
                }
            });
            
        }
</script>
</html>
```

- index.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
<p>你好！[[${user.username}]]</p>
<a th:href="@{/logoudt}">注销</a>
</body>
</html>
```

