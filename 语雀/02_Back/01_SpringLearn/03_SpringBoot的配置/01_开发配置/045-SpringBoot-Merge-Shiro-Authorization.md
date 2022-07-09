# SpringBoot-Merge-Shiro-Authorization

- 授权也称为权限控制，是管理资源访问的过程，即根据不同用户权限判断是否有访问相应资源的权限，在Shiro中，权限控制有三个核心的元素：权限，角色，用户。

- 在这里我们使用`RBAC`(基于角色的权限控制）模型设置用户，角色和权限之间的关系，简单的说，一个用户有若干角色，每一个角色拥有若干权限，这样就构成了用户角色权限模型，在这样的模型中，用户和角色之间，角色和权限之间，一般是多对多的关系。

# 1、Need Config

- `#doGetAuthorizationInfo()`

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
        UmsAdmin user = (UmsAdmin) SecurityUtils.getSubject().getPrincipal();
        String userName = user.getUsername();

        System.out.println("用户" + userName + "获取权限-----ShiroRealm.doGetAuthorizationInfo");
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();

        //1.获取用户角色集
        List<UmsRole> roleList = adminService.findRolesByUsername(userName);
        
        Set<String> roleSet = new HashSet<String>();
        
        for (UmsRole r : roleList) {
            roleSet.add(r.getName());
        }
        
        
        simpleAuthorizationInfo.setRoles(roleSet);

        //2.获取用户权限集
        List<UmsPermission> permissionList = adminService.findPermissionByUsername(userName);
        Set<String> permissionSet = new HashSet<String>();
       
        for (UmsPermission p : permissionList) {
            permissionSet.add(p.getValue());
        }
        
        
        simpleAuthorizationInfo.setStringPermissions(permissionSet);

        return simpleAuthorizationInfo;

    }
    


    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {

        String userName = (String) token.getPrincipal();
        String password = new String((char[]) token.getCredentials());

        System.out.println("用户" + userName + "认证-----ShiroRealm.doGetAuthenticationInfo");


        UmsAdmin user = adminService.findByUsername(userName);

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

- ShiroConfig

```java
@Configuration
public class ShiroConfig {

    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(SecurityManager securityManager,ShiroRealm shiroRealm) {

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
        map.put("/**", "user");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(map);

        return shiroFilterFactoryBean;
    }


    @Bean
    public SecurityManager securityManager(CookieRememberMeManager rememberMeManager){
        DefaultWebSecurityManager securityManager =  new DefaultWebSecurityManager();
        
        securityManager.setRealm(shiroRealm);
        securityManager.setRememberMeManager(rememberMeManager);
        
        return (SecurityManager) securityManager;
    }


    @Bean
    public ShiroRealm shiroRealm() {
        ShiroRealm shiroRealm = new ShiroRealm();
        return shiroRealm;
    }


	@Bean
    public SimpleCookie rememberMeCookie() {
        SimpleCookie cookie = new SimpleCookie("rememberMe");
        cookie.setMaxAge(86400);
        return cookie;
    }
    
    @Bean
    public CookieRememberMeManager rememberMeManager(SimpleCookie rememberMeCookie) {
        CookieRememberMeManager cookieRememberMeManager = new CookieRememberMeManager();
        cookieRememberMeManager.setCookie(rememberMeCookie);
        cookieRememberMeManager.setCipherKey(Base64.decode("4AvVhmFLUs0KTA3Kprsdag=="));
        return cookieRememberMeManager;
    }


	//1.开启接口权限访问校验
    @Bean
    @ConditionalOnMissingBean
    public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator() {
        DefaultAdvisorAutoProxyCreator defaultAAP = new DefaultAdvisorAutoProxyCreator();
        defaultAAP.setProxyTargetClass(true);
        return defaultAAP;
    }
    //1.开启接口权限访问校验
    @Bean
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(SecurityManager securityManager) {
        AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor = new AuthorizationAttributeSourceAdvisor();
        authorizationAttributeSourceAdvisor.setSecurityManager(securityManager);
        return authorizationAttributeSourceAdvisor;
    }

}
```

- GlobalExceptionHandler

```java
@ControllerAdvice
@Order(value = Ordered.HIGHEST_PRECEDENCE)
public class GlobalExceptionHandler {
    
    @ExceptionHandler(value = AuthorizationException.class)
    public String handleAuthorizationException() {
        return "403";
    }
}
```

- 403.html

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>暂无权限</title>
</head>
<body>
<p>您没有权限访问该资源！！</p>
<a th:href="@{/index}">返回</a>
</body>
```

# 2、User Anno

```java
package com.wnx.mall.tiny.controller;

import org.apache.shiro.authz.annotation.RequiresPermissions;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/user")
public class UserController {

    @RequiresPermissions("user:user")
    @RequestMapping("list")
    public String userList(Model model) {
        model.addAttribute("value", "获取用户信息");
        return "user";
    }

    @RequiresPermissions("user:add")
    @RequestMapping("add")
    public String userAdd(Model model) {
        model.addAttribute("value", "新增用户");
        return "user";
    }

    @RequiresPermissions("user:delete")
    @RequestMapping("delete")
    public String userDelete(Model model) {
        model.addAttribute("value", "删除用户");
        return "user";
    }
    
}

```





