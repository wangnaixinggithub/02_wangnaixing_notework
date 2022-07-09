# SpringBoot-Merge-Shiro-OnlineManager

# 1、One Way:EhCache

## 1、Need Config

> 配置SessionDao,配置Session会话管理器,注入到SecurityManager安全管理器中

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
        map.put("/**", "user");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(map);

        return shiroFilterFactoryBean;
    }


    @Bean
    public SecurityManager securityManager(ShiroRealm shiroRealm,
                                           CookieRememberMeManager rememberMeManager,
                                           EhCacheManager ehCacheManager,
                                           SessionManager sessionManager){
        DefaultWebSecurityManager securityManager =  new DefaultWebSecurityManager();
        
        securityManager.setRealm(shiroRealm);
        securityManager.setRememberMeManager(rememberMeManager);
        securityManager.setCacheManager(ehCacheManager;
        securityManager.setSessionManager(sessionManager;
                                          
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
        cookieRememberMeManager.setCookie(rememberMeCookie;
        cookieRememberMeManager.setCipherKey(Base64.decode("4AvVhmFLUs0KTA3Kprsdag=="));
       
        return cookieRememberMeManager;
    }




    @Bean
    @ConditionalOnMissingBean
    public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator() {
        DefaultAdvisorAutoProxyCreator defaultAAP = new DefaultAdvisorAutoProxyCreator();
        defaultAAP.setProxyTargetClass(true);
        return defaultAAP;
    }

    @Bean
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(SecurityManager securityManager) {
        AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor = new AuthorizationAttributeSourceAdvisor();
        authorizationAttributeSourceAdvisor.setSecurityManager(securityManager);
        return authorizationAttributeSourceAdvisor;
    }

    @Bean
    public EhCacheManager ehCacheManager() {
        EhCacheManager em = new EhCacheManager();
        em.setCacheManagerConfigFile("classpath:config/shiro-ehcache.xml");
        return em;
    }

    @Bean
    public ShiroDialect shiroDialect() {
        return new ShiroDialect();
    }


    /**
     * 配置SessionDao
     * @return
     */
    @Bean
    public SessionDAO sessionDAO() {
        MemorySessionDAO sessionDAO = new MemorySessionDAO();
        return sessionDAO;
    }

    /**
     * 配置Session会话管理器
     * @return
     */
    @Bean
    public SessionManager sessionManager() {
        DefaultWebSessionManager sessionManager = new DefaultWebSessionManager();
        Collection<SessionListener> listeners = new ArrayList<SessionListener>();
        listeners.add(new ShiroSessionListener());
        sessionManager.setSessionListeners(listeners);
        sessionManager.setSessionDAO(sessionDAO());
        return sessionManager;
    }



}
```

```java
package com.wnx.mall.tiny.component;

import org.apache.shiro.session.Session;
import org.apache.shiro.session.SessionListener;

import java.util.concurrent.atomic.AtomicInteger;

public class ShiroSessionListener implements SessionListener {
    private final AtomicInteger sessionCount = new AtomicInteger(0);
    @Override
    public void onStart(Session session) {
        sessionCount.incrementAndGet();
    }

    @Override
    public void onStop(Session session) {
        sessionCount.decrementAndGet();
    }

    @Override
    public void onExpiration(Session session) {
        sessionCount.decrementAndGet();
    }
}

```

## 2、Your Business

- 查看在线人数以及在线用户管理
- 让当前在线用户下线

```java
package com.wnx.mall.tiny.dto;

import lombok.Data;

import java.io.Serializable;
import java.util.Date;

@Data
public class UserOnline implements Serializable {
    //session id
    private String id;
    //用户id
    private String userId;
    //用户名称
    private String username;
    //用户主机地址
    private String host;
    //用户登录时系统IP
    private String systemHost;
    //状态
    private String status;
    //session创建时间
    private Date startTimestamp;
    //session最后访问时间
    private Date lastAccessTime;
    //超时时间
    private Long timeout;

}

```

```java

public interface SessionService {
    /***
     * 查看所有在线用户
     * @return
     */
    List<UserOnline> list();

    /**
     * 根据SessionId踢出用户
     * @param sessionId
     * @return
     */
    boolean forceLogout(String sessionId);

}

```

```java
@Service
public class SessionServiceImpl implements SessionService {
    @Resource
    private SessionDAO sessionDAO;

    @Override
    public List<UserOnline> list() {
        List<UserOnline> list = new ArrayList<>();
        Collection<Session> sessions = sessionDAO.getActiveSessions();
        for (Session session : sessions) {
            UserOnline userOnline = new UserOnline();
            UmsAdmin user = new UmsAdmin();
            SimplePrincipalCollection principalCollection = new SimplePrincipalCollection();
            if (session.getAttribute(DefaultSubjectContext.PRINCIPALS_SESSION_KEY) == null) {
                continue;
            } else {
                principalCollection = (SimplePrincipalCollection) session
                        .getAttribute(DefaultSubjectContext.PRINCIPALS_SESSION_KEY);
                
                user = (UmsAdmin) principalCollection.getPrimaryPrincipal();
                
                userOnline.setUsername(user.getUsername());
                userOnline.setUserId(user.getId().toString());
            }
            userOnline.setId((String) session.getId());
            userOnline.setHost(session.getHost());
            userOnline.setStartTimestamp(session.getStartTimestamp());
            userOnline.setLastAccessTime(session.getLastAccessTime());
           
            long timeout = session.getTimeout();
            if (timeout == 0L) {
                userOnline.setStatus("离线");
            } else {
                userOnline.setStatus("在线");
            }
            userOnline.setTimeout(timeout);
           
            list.add(userOnline);
        }
        
        return list;
    }

    @Override
    public boolean forceLogout(String sessionId) {
        Session session = sessionDAO.readSession(sessionId);
        session.setTimeout(0);
        return true;
    }
}

```

```java

@Controller
@RequestMapping("/online")
public class SessionController {
    @Resource
    SessionService sessionService;

    @RequestMapping("index")
    public String online() {
        return "online";
    }

    @ResponseBody
    @RequestMapping("list")
    public List<UserOnline> list() {
        return sessionService.list();
    }

    @ResponseBody
    @RequestMapping("forceLogout")
    public CommonResult forceLogout(String id) {
        try {
            sessionService.forceLogout(id);
            return CommonResult.success("");
        } catch (Exception e) {
            e.printStackTrace();
            return CommonResult.failed("踢出用户失败");
        }
    }
}

```

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>在线用户管理</title>
    <script th:src="@{/js/jquery-3.3.1.js}"></script>
    <script th:src="@{/js/dateFormat.js}"></script>
</head>
<body>
<h3>在线用户数：<span id="onlineCount"></span></h3>
<table>
    <tr>
        <th>序号</th>
        <th>用户名称</th>
        <th>登录时间</th>
        <th>最后访问时间</th>
        <th>主机</th>
        <th>状态</th>
        <th>操作</th>
    </tr>
</table>
<a th:href="@{/index}">返回</a>
</body>
<script th:inline="javascript">
        $.get("http://localhost:8080/online/list", {}, function(r){
            var length = r.length;
            $("#onlineCount").text(length);
            var html = "";
            for(var i = 0; i < length; i++){
                html += "<tr>"
                    + "<td>" + (i+1) + "</td>"
                    + "<td>" + r[i].username + "</td>"
                    + "<td>" + new Date(r[i].startTimestamp).Format("yyyy-MM-dd hh:mm:ss") + "</td>"
                    + "<td>" + new Date(r[i].lastAccessTime).Format("yyyy-MM-dd hh:mm:ss") + "</td>"
                    + "<td>" + r[i].host + "</td>"
                    + "<td>" + r[i].status + "</td>"
                    + "<td><a href='#' onclick='offline(\"" + r[i].id + "\",\"" + r[i].status +"\")'>下线</a></td>"
                    + "</tr>";
            }
            $("table").append(html);
        },"json");

        function offline(id,status){
            if(status === "离线"){
                alert("该用户已是离线状态！！");
                return;
            }
            $.get("http://localhost:8080/online/forceLogout", {"id": id}, function(r){
                if (r.code === 200) {
                    alert('该用户已强制下线！');
                    location.href = 'http://localhost:8080/online/index';
                } else {
                    alert(r.message);
                }
            },"json");
        }
</script>
</html>
```

# 2、Another Way: Redis

## 1、Need Config

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
        map.put("/**", "user");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(map);

        return shiroFilterFactoryBean;
    }


    @Bean
    public SecurityManager securityManager(ShiroRealm shiroRealm,
                                           CookieRememberMeManager rememberMeManager,
                                           EhCacheManager ehCacheManager,
                                           SessionManager sessionManager){
        DefaultWebSecurityManager securityManager =  new DefaultWebSecurityManager();
        
        securityManager.setRealm(shiroRealm);
        securityManager.setRememberMeManager(rememberMeManager);
        securityManager.setCacheManager(ehCacheManager;
        securityManager.setSessionManager(sessionManager;
                                          
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
        cookieRememberMeManager.setCookie(rememberMeCookie;
        cookieRememberMeManager.setCipherKey(Base64.decode("4AvVhmFLUs0KTA3Kprsdag=="));
       
        return cookieRememberMeManager;
    }




    @Bean
    @ConditionalOnMissingBean
    public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator() {
        DefaultAdvisorAutoProxyCreator defaultAAP = new DefaultAdvisorAutoProxyCreator();
        defaultAAP.setProxyTargetClass(true);
        return defaultAAP;
    }

    @Bean
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(SecurityManager securityManager) {
        AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor = new AuthorizationAttributeSourceAdvisor();
        authorizationAttributeSourceAdvisor.setSecurityManager(securityManager);
        return authorizationAttributeSourceAdvisor;
    }

   
    /**
     * redis管理器
     * @return
     */
    public RedisManager redisManager() {
        RedisManager redisManager = new RedisManager();
        return redisManager;
    }

    /**
     * 配置方言标签
     * @return
     */
    @Bean
    public ShiroDialect shiroDialect() {
        return new ShiroDialect();
    }

    /**
     * redis缓存管理器
     * @return
     */
    public RedisCacheManager cacheManager() {
        RedisCacheManager redisCacheManager = new RedisCacheManager();
        redisCacheManager.setRedisManager(redisManager());
        return redisCacheManager;
    }

    /**
     * Redis版会话DAO
     * @return
     */
    @Bean
    public RedisSessionDAO sessionDAO() {
        RedisSessionDAO redisSessionDAO = new RedisSessionDAO();
        redisSessionDAO.setRedisManager(redisManager());
        return redisSessionDAO;
    }

    /**
     * 会话管理器
     * @return
     */
    @Bean
    public SessionManager sessionManager() {
        DefaultWebSessionManager sessionManager = new DefaultWebSessionManager();
        Collection<SessionListener> listeners = new ArrayList<SessionListener>();
        listeners.add(new ShiroSessionListener());
        sessionManager.setSessionListeners(listeners);
        sessionManager.setSessionDAO(sessionDAO());
        return sessionManager;
    }


}
```

```java
package com.wnx.mall.tiny.component;

import org.apache.shiro.session.Session;
import org.apache.shiro.session.SessionListener;

import java.util.concurrent.atomic.AtomicInteger;

public class ShiroSessionListener implements SessionListener {
    private final AtomicInteger sessionCount = new AtomicInteger(0);
    @Override
    public void onStart(Session session) {
        sessionCount.incrementAndGet();
    }

    @Override
    public void onStop(Session session) {
        sessionCount.decrementAndGet();
    }

    @Override
    public void onExpiration(Session session) {
        sessionCount.decrementAndGet();
    }
}

```

## 2、Your Business

```java
package com.wnx.mall.tiny.service;

import com.wnx.mall.tiny.dto.UserOnline;

import java.util.List;

public interface SessionService {
    /***
     * 查看所有在线用户
     * @return
     */
    List<UserOnline> list();

    /**
     * 根据SessionId踢出用户
     * @param sessionId
     * @return
     */
    boolean forceLogout(String sessionId);

}

```

```java
package com.wnx.mall.tiny.service.impl;

import com.wnx.mall.tiny.dto.UserOnline;
import com.wnx.mall.tiny.mbg.model.UmsAdmin;
import com.wnx.mall.tiny.service.SessionService;
import org.apache.shiro.session.Session;
import org.apache.shiro.session.mgt.eis.SessionDAO;
import org.apache.shiro.subject.SimplePrincipalCollection;
import org.apache.shiro.subject.support.DefaultSubjectContext;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;
import java.util.ArrayList;
import java.util.Collection;
import java.util.List;
@Service
public class SessionServiceImpl implements SessionService {
    @Resource
    private SessionDAO sessionDAO;

    @Override
    public List<UserOnline> list() {
        List<UserOnline> list = new ArrayList<>();
        Collection<Session> sessions = sessionDAO.getActiveSessions();
        for (Session session : sessions) {
            UserOnline userOnline = new UserOnline();
            UmsAdmin user = new UmsAdmin();
            SimplePrincipalCollection principalCollection = new SimplePrincipalCollection();
            if (session.getAttribute(DefaultSubjectContext.PRINCIPALS_SESSION_KEY) == null) {
                continue;
            } else {
                principalCollection = (SimplePrincipalCollection) session
                        .getAttribute(DefaultSubjectContext.PRINCIPALS_SESSION_KEY);
                user = (UmsAdmin) principalCollection.getPrimaryPrincipal();
                userOnline.setUsername(user.getUsername());
                userOnline.setUserId(user.getId().toString());
            }
            userOnline.setId((String) session.getId());
            userOnline.setHost(session.getHost());
            userOnline.setStartTimestamp(session.getStartTimestamp());
            userOnline.setLastAccessTime(session.getLastAccessTime());
            long timeout = session.getTimeout();
            if (timeout == 0L) {
                userOnline.setStatus("离线");
            } else {
                userOnline.setStatus("在线");
            }
            userOnline.setTimeout(timeout);
            list.add(userOnline);
        }
        return list;
    }

    @Override
    public boolean forceLogout(String sessionId) {
        Session session = sessionDAO.readSession(sessionId);
        sessionDAO.delete(session);
        return true;
    }
}

```

