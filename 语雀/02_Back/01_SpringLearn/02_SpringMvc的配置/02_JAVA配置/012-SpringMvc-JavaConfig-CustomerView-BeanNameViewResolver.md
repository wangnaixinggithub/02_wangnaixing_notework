# SpringMvc-JavaConfig-CustomerView-BeanNameViewResolver

## 1、BeanNameViewResolver处理beanName视图

```java
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
    <mvc:annotation-driven/>
    <bean id="httpMessageConverter" class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
        <!--
            1.使用自定义的ObjectMapper
        -->
        <property name="objectMapper" ref="customObjectMapper"/>
    </bean>
    <!--
        2.注入到消息转换器集合中
    -->
    <mvc:annotation-driven>
        <mvc:message-converters>
            <ref bean="httpMessageConverter"/>
        </mvc:message-converters>
    </mvc:annotation-driven>
    
    <bean id="validatorFactoryBean" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
        <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
    </bean>
    <bean id="viewResolver" class="org.springframework.web.servlet.view.BeanNameViewResolver"/>
</beans>
```

## 2、实现View接口

```java
package com.wnx.springmvc.view;
import org.springframework.web.servlet.View;

@Component
public class CustomerView implements View {
    @Override
    public void render(Map<String, ?> model, @NonNull HttpServletRequest request, @NonNull HttpServletResponse response) throws Exception {
        response.setContentType("text/html; charset=UTF-8");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().println("<h1>王乃醒的视图<h1></br>");
        response.getWriter().println("<h3>WelCome To WangNaiXing World!</h3>");
        response.getWriter().close();
    }

    @Override
    public String getContentType() {
        return MediaType.TEXT_HTML_VALUE;
    }
}

```

## 3、使用WangNaixingView

> 有下方接口，返回beanName作为方法返回值

```java
@Log4j2
@Controller
public class UserController{
 	@GetMapping("/time")
    public String time(@DateTimeFormat(pattern = "yyyy-MM-dd") Date birth){
        log.info("Date=>{}",birth);
        return "customerView";
    }
}
```

## 4、渲染后响应

> 视图解析器进行解析beanName,渲染WangnaixingView，响应给浏览器。

![image-20220509230800714](060-%E9%9B%B7%E4%B8%B0%E9%98%B3-SpringMVC%E8%A7%86%E5%9B%BE%E8%A7%A3%E6%9E%90%E5%99%A8.assets/image-20220509230800714.png)