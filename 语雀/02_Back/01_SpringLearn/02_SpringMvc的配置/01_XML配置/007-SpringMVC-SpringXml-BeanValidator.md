# SpringMVC-SpringXml-BeanValidator

# 1、配置

```xml
        <dependency>
            <groupId>org.hibernate.validator</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>6.1.0.Final</version>
        </dependency>
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
    <context:component-scan base-package="com.wnx.springmvc" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <mvc:annotation-driven/>
    <mvc:resources mapping="/**" location="/"/>
    
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

# 2、怎么用？

```java

@AllArgsConstructor
@NoArgsConstructor
@Data
public class Student implements Serializable {
    @NotNull
    private Integer id;
    @NotBlank
    @Size(min = 2, max = 10)
    private String name;
    @Email
    @NotBlank
    private String email;
    @NotNull
    @Max(150)
    private Integer age;
}

```

# 3、异常封装在BindingResult

```java
@Log4j2
@Controller
public class BeanValidationController {
    
    @PostMapping("/addStudent")
    public void handleFormSubmit(@Validated Student student, BindingResult bindingResult){
        if (bindingResult != null){
            List<ObjectError> allErrors = bindingResult.getAllErrors();
            allErrors.forEach(error-> log.info("ObjectName => {},defaultMessage => {}",error.getObjectName(),error.getDefaultMessage()));
        }
    }

}
```

```c#
[2022-05-08 16:13:56:845] [INFO] - com.wnx.springmvc.controller.BeanValidationController.lambda$handleFormSubmit$0(BeanValidationController.java:20) - ObjectName => student,defaultMessage => 不能为空
[2022-05-08 16:13:56:845] [INFO] - com.wnx.springmvc.controller.BeanValidationController.lambda$handleFormSubmit$0(BeanValidationController.java:20) - ObjectName => student,defaultMessage => 不能为空
[2022-05-08 16:13:56:845] [INFO] - com.wnx.springmvc.controller.BeanValidationController.lambda$handleFormSubmit$0(BeanValidationController.java:20) - ObjectName => student,defaultMessage => 不能为null
[2022-05-08 16:13:56:845] [INFO] - com.wnx.springmvc.controller.BeanValidationController.lambda$handleFormSubmit$0(BeanValidationController.java:20) - ObjectName => student,defaultMessage => 不能为null
```

# 4、校验组

- 定义校验组1，校验组2

```java
package validationgruop;
public interface ValidationGroup1 {
}

```

```java
package validationgruop;

public interface ValidationGroup2 {
}

```

- 1·指定POJO字段使用哪一个校验组的规则

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
public class Student implements Serializable {
    @NotNull(groups = {ValidationGroup1.class, ValidationGroup2.class})
    private Integer id;
    
    @NotBlank(groups = {ValidationGroup1.class, ValidationGroup2.class})
    @Size(min = 2, max = 10,groups = {ValidationGroup1.class, ValidationGroup2.class})
    private String name;
    
    @Email(groups = {ValidationGroup1.class, ValidationGroup2.class})
    @NotBlank(groups = {ValidationGroup1.class, ValidationGroup2.class})
    private String email;
   
    /**
     * 该年龄字段只属于检验组1，校验组2不会进行校验
     */
    @NotNull(groups = ValidationGroup1.class)
    @Max(value = 150,groups = ValidationGroup1.class)
    private Integer age;
}

```

- 只使用检验组2

> 这样规则下，年龄字段不会被校验。

```java
@Controller
public class BeanValidationController {
    @GetMapping("/addStudent")
    public ModelAndView addStudent(){
        ModelAndView mv = new ModelAndView("addStudent");
        return mv;
    }
    @PostMapping("/addStudent")
    public void handleFormSubmit(@Validated(ValidationGroup2.class)  Student student, BindingResult bindingResult){
            if (bindingResult != null){
                List<ObjectError> allErrors = bindingResult.getAllErrors();
                allErrors.forEach(error->{
                    System.out.println(error.getObjectName()+":"+error.getDefaultMessage());
                });
            }
    }
}
```

