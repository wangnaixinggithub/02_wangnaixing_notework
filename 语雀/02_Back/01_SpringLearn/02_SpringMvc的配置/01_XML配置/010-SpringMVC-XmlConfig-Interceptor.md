# SpringMVC-XmlConfig-Interceptor

## 1、配置拦截器

```java
@Component
public class MyInterceptor1 implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("MyInterceptor1...preHandle");
        return true;
    }
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("MyInterceptor1...postHandle");
    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("MyInterceptor1...afterCompletion");
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/mvc
	http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="defaultEncoding" value="UTF-8"/>
        <property name="maxUploadSize" value="10485760"/>
        <property name="maxUploadSizePerFile" value="10485760"/>
        <property name="maxInMemorySize" value="4096"/>
        <property name="uploadTempDir" value="/upload/temp"/>
    </bean>
    <context:component-scan base-package="com.wnx.springmvc" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <mvc:annotation-driven/>
    <mvc:resources mapping="/**" location="/"/>


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
    <bean id="validatorFactoryBean" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
        <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
    </bean>

    <!--
        配置拦截器
         path:拦截什么路径
         并使用哪个拦截器来处理
    -->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <ref bean="myInterceptor1"/>
        </mvc:interceptor>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <ref bean="myInterceptor2"/>
        </mvc:interceptor>
    </mvc:interceptors>
</beans>
```

## 2、两个拦截器的执行流程

> 配置中，myInterceptor1在myInterceptor2之前，所以先走myInterceptor1.

```java
com.wnx.spring.filter.FirstHandlerInterceptorpreHandle..............
com.wnx.spring.filter.SecondHandlerInterceptorpreHandle..............
Handler方法执行了哦..............
com.wnx.spring.filter.SecondHandlerInterceptorpostHandle..............
com.wnx.spring.filter.FirstHandlerInterceptorpostHandle..............
com.wnx.spring.filter.SecondHandlerInterceptorafterCompletion..............
com.wnx.spring.filter.FirstHandlerInterceptorafterCompletion..............
```