# SpringMvc-BeanNameUrlHandlerMapping-SimpleControllerHandlerAdapter

## 1、handler实现`org.springframework.web.servlet.mvc.Controller`

```java
@Log4j2
@Controller("/addUser")
public class UserController implements org.springframework.web.servlet.mvc.Controller {

    private final UserService userService;
    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }


    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        log.info("UserController...execute...");
        userService.addUser();
        ModelAndView modelAndView = new ModelAndView("hello");
        modelAndView.addObject("name","王乃醒");
        return modelAndView;
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

## 2、BeanNameUrlHandlerMapping And SimpleControllerHandlerAdapter

> 处理器映射器：使用beanName和Url绑定的处理器映射器，BeanNameUrlHandlerMapping.
>
> 处理器适配器：处理实现Controller接口的处理适配器，SimpleControllerHandlerAdapter

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

    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping">
        <property name="beanName" value="/addUser"/>
    </bean>
    <bean  id="handlerAdapter" class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
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

