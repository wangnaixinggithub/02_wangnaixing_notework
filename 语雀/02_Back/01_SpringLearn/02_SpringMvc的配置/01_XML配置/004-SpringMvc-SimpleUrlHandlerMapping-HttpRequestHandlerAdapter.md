# SpringMvc-SimpleUrlHandlerMapping-HttpRequestHandlerAdapter

## 1、handler实现`org.springframework.web.HttpRequestHandler`

```java
@Log4j2
@Controller
public class UserController implements HttpRequestHandler {

    private final UserService userService;
    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }


    @SneakyThrows
    @Override
    public void handleRequest(HttpServletRequest request, HttpServletResponse response) {
        log.info("UserController...execute...");
        userService.addUser();
        response.setContentType("text/html; charset=UTF-8");
        response.getWriter().println("王乃醒是大帅哥！！");
        response.getWriter().flush();
        response.getWriter().close();
    }
}


@Service
@Log4j2
public class UserService {
    private final UserDao userDao;

    @Autowired
    public UserService(UserDao userDao) {
        this.userDao = userDao;
    }

    public void addUser() {
        log.info("UserService...execute...");
        userDao.addUser();
    }
}

@Repository
@Log4j2
public class UserDao {
    public void addUser() {
        log.info("UserDao...execute...");
        log.info("系统正在添加用户信息...");
    }
}

```

```html
/webapp/WEB-INF/hello.html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>SpringMVC入门-XmlConfig</title>
</head>
<body>
<h1 th:text="${name}" ></h1>
</body>
</html>
```



# 2、SimpleUrlHandlerMapping And HttpRequestHandlerAdapter

> 处理器映射器：使用beanName和Url维护一个prop的处理器映射器，SimpleUrlHandlerMapping.
>
> 处理器适配器：处理实现HttpRequestHandler接口的处理适配器，HttpRequestHandlerAdapter

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/mvc
	http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.wnx.springmvc" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <mvc:default-servlet-handler/>
    <bean id="handlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
        <property name="mappings">
            <props>
                <prop key="/addUser">userController</prop>
            </props>
        </property>
    </bean>
    <bean  id="handlerAdapter" class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"/>
    <bean id="templateResolver" class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
        <property name="prefix" value="/WEB-INF/"/>
        <property name="suffix" value=".html"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="order" value="1"/>
        <property name="templateMode" value="HTML"/>
        <property name="cacheable" value="false"/>
    </bean>
    <bean id="templateEngine" class="org.thymeleaf.spring5.SpringTemplateEngine">
        <property name="templateResolver" ref="templateResolver"/>
    </bean>
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="templateEngine" ref="templateEngine"/>
        <property name="characterEncoding" value="UTF-8"/>
    </bean>
</beans>
```