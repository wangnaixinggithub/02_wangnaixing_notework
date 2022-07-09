# SpringMvc-XmlConfig-CommonsMultipartResolver

# 1、CommonsMultipartResolver处理文件上传

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
    <mvc:annotation-driven/>
    <!--配置通用Multipart解析器-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="defaultEncoding" value="UTF-8"/>
        <property name="maxUploadSizePerFile" value="1024000"/>
        <property name="maxUploadSize" value="1024000"/>
        <property name="uploadTempDir" value="/WEB-INF/temp"/>
        <property name="maxInMemorySize" value="102400"/>
    </bean>
    <bean id="httpMessageConverter" class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
        <property name="objectMapper" ref="customObjectMapper"/>
    </bean>
    <mvc:annotation-driven>
        <mvc:message-converters>
            <ref bean="httpMessageConverter"/>
        </mvc:message-converters>
    </mvc:annotation-driven>
    
    <bean id="validatorFactoryBean" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
        <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
    </bean>
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

# 2、处理`MultipartFile`类的文件。

```java
    @SneakyThrows
    @PostMapping("/upload")
    public String upload(@RequestParam("desc")String desc,@RequestParam("file") MultipartFile file){
        System.out.println("desc->"+desc);
        System.out.println("file->"+file.getOriginalFilename());
        //一行代码搞定文件上传。
        file.transferTo(new File("D:\\my_workspace\\"+file.getOriginalFilename()));
        return "success";
    }
```

