# SpringMvc-web.xml

**它主要的是用来配置欢迎页、servlet、filter、listener等以及定制servlet、JSP、Context初始化参数。**

**加载顺序:context-param–> listener –> filter –> servlet**

## web.xml其他的标签

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

