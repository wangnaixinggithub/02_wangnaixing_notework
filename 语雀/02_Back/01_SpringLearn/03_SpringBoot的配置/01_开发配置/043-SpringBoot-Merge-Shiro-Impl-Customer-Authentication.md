# SpringBoot-Merge-Shiro-Impl-Customer-Authentication

在Spring Boot中集成Shiro进行用户的认证过程主要可以归纳为以下三点：

1、定义一个ShiroConfig,然后配置SecurityManager Bean,SecurityManager为Shiro的安全管理器，管理着所有的Subject.

2、在ShiroConfig中配置ShiroFilterFactoryBean,其为Shiro过滤器工厂类，依赖于SecurityManager

3、自定义Realm实现，自定义Realm实现，Realm包含`doGetAuthorizationInfo()`和`doGetAuthenticationInfo()`方法，因为本文只涉及用户认证，所以只实现`doGetAuthenticationInfo()`方法。

# 1、Need Config

```xml
	<!-- pom.xml -->
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.1.3.RELEASE</version>
            <relativePath/> 
        </parent>

        <!--shiro-->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring</artifactId>
            <version>1.6.0</version>
        </dependency>

```

```yaml
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

- ShiroConfig

```java
@Configuration
public class ShiroConfig {

    //1.Filter工厂
    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(SecurityManager securityManager) {

        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();

        // 设置securityManager
        shiroFilterFactoryBean.setSecurityManager(securityManager);
       
        // 登录的url
        shiroFilterFactoryBean.setLoginUrl("/login");
        
        // 登录成功后跳转的url
        shiroFilterFactoryBean.setSuccessUrl("/index");
        
        // 未授权url
        shiroFilterFactoryBean.setUnauthorizedUrl("/403");

		
        LinkedHashMap<String, String> map = new LinkedHashMap<>();
        map.put("/css/**", "anon");
        map.put("/js/**", "anon");
        map.put("/fonts/**", "anon");
        map.put("/img/**", "anon");
        map.put("/druid/**", "anon");
        map.put("/logout", "logout");
        map.put("/", "anon");
        map.put("/**", "authc");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(map);

        return shiroFilterFactoryBean;
    }


    //权限管理，配置主要是Realm的管理认证
    @Bean
    public SecurityManager securityManager(ShiroRealm shiroRealm){
        DefaultWebSecurityManager securityManager =  new DefaultWebSecurityManager();
      
        securityManager.setRealm(shiroRealm);
        
        return (SecurityManager) securityManager;
    }



    //将自己的验证方式加入容器
    @Bean
    public ShiroRealm shiroRealm() {
        return  new ShiroRealm();
    }

}
```

- ShiroRealm

```java
public class ShiroRealm extends AuthorizingRealm {

    @Resource
    private UmsAdminService adminService;

    /**
     * 获取用户角色和权限
     * @param principalCollection
     * @return
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    /**
     * 登录认证
     * @param token
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //1.从Token中，获取用户输入的用户名和密码。
        String userName = (String) token.getPrincipal();
        String password = new String((char[]) token.getCredentials());

        System.out.println("用户" + userName + "认证-----ShiroRealm.doGetAuthenticationInfo");

        //2.通过用户名到数据库查询用户信息
        UmsAdmin user = adminService.findByUsername(userName);
		
        //3.Do Handle
        if (user == null) {
            throw new UnknownAccountException("用户名或密码错误！");
        }
		
        if (!(user.getPassword().equals(password))){
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

- MD5Utils

```java
package com.wnx.mall.tiny.utils;

import org.apache.shiro.crypto.hash.SimpleHash;
import org.apache.shiro.util.ByteSource;

public class MD5Utils {
    private static final String SALT = "mrbird";

    private static final String ALGORITH_NAME = "md5";

    private static final int HASH_ITERATIONS = 2;

    public static String encrypt(String pswd) {
        String newPassword = new SimpleHash(ALGORITH_NAME, pswd, ByteSource.Util.bytes(SALT), HASH_ITERATIONS).toHex();
        return newPassword;
    }

    public static String encrypt(String username, String pswd) {
        String newPassword = new SimpleHash(ALGORITH_NAME, pswd, ByteSource.Util.bytes(username + SALT),
                HASH_ITERATIONS).toHex();
        return newPassword;
    }
    public static void main(String[] args) {

        System.out.println(MD5Utils.encrypt("admin", "123456"));
    }
}

```

# 2、Your Business

- LoginController

```java
@Controller
public class LoginController {
    //跳转登录页
    @GetMapping("/login")
    public String login() {
        return "login";
    }
	
 	//跳转首页
    @RequestMapping({"/index"，"/"})
    public String index(Model model) {
        // SecurityUtils 获取登录主体信息
        UmsAdmin user = (UmsAdmin) SecurityUtils.getSubject().getPrincipal();
        
        model.addAttribute("user", user);
        
        return "index";
    }

	
    @ResponseBody
    @PostMapping("/login")
    public CommonResult login(String username, String password) {
       
        //1.密码MD5加密
        password = MD5Utils.encrypt(username, password);
        
        //2.根据用户名和密码构建 UsernamePasswordToken
        UsernamePasswordToken token = new UsernamePasswordToken(username, password);
       
        //3.获取Subject对象
        Subject subject = SecurityUtils.getSubject();

        try {
            //4.执行登录操作
            subject.login(token);
            
            //5.登录成功，返回成功结果。
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
        <button onclick="login()">登录</button>
    </div>
</div>
</body>
<script th:inline="javascript">
        function login() {
            var username = $("input[name='username']").val();
            var password = $("input[name='password']").val();
           
            $.ajax({
                type: "post",
                url:  "http://localhost:8080/login",
                data: {"username": username, "password": password},
                dataType: "json",
                success: function (res) {
                    if (res.code == 200) {
                        location.href = 'http://localhost:8080/index';
                    } else {
                        alert(res.message);
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

# 3、Validaton Test

```java
http://localhost:8080/login
```

- 密码不正确
- 用户不存在
- 成功登录