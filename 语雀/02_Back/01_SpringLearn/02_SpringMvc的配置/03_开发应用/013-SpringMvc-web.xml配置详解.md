# Spring MVC的web.xml

## 一、web.xml

首先java Web项目中的并不是必须需要web.xml文件的。

其次**它主要的是用来配置欢迎页、servlet、filter、listener等以及定制servlet、JSP、Context初始化参数。**

最后加载顺序

**context-param–> listener –> filter –> servlet**

## 二、配置SpringMvc

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
<!--    1.加载Spring父容器-->
        <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring.xml</param-value>
        </context-param>
        <listener>
            <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
        </listener>

    <!-- 2.加载子容器 -->
    <servlet>
        <servlet-name>spring-mvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>spring-mvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--3.配置字符编码过滤器-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!--4.配置支持PUT DELETE请求-->
    <filter>
        <filter-name>hiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>hiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

![](013-SpringMvc-web.xml%E9%85%8D%E7%BD%AE%E8%AF%A6%E8%A7%A3.assets/image-20220511213956960.png)



## 三、web.xml其他的标签

（1）标识项目的名称

```xml
<display-name>SpringMVC</display-name>
```

（2）欢迎页

```xml
<welcome-file-list>
    <welcome-file>login.jsp</welcome-file>
</welcome-file-list>
```

（3）错误页

```xml
<!-- 后台程序异常错误跳转页面 -->
<error-page> 
  <exception-type>java.lang.Throwable</exception-type> 
  <location>WEB-INF/error.jsp</location> 
</error-page> 

<!-- 500跳转页面-->
<error-page> 
  <error-code>500</error-code> 
  <location>/WEB-INF/500.jsp</location> 
</error-page> 

<!-- 404跳转页面 -->
<error-page> 
  <error-code>404</error-code> 
  <location>/WEB-INF/404.jsp</location> 
</error-page>
```

