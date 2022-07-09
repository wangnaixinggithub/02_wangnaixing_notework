# SpringMVC-JavaConfig-BeanValidator

# 1、配置

```xml
        <dependency>
            <groupId>org.hibernate.validator</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>6.1.0.Final</version>
        </dependency>
```

```java
@Configuration
@ComponentScan(basePackages = "com.wnx.springmvc",
        useDefaultFilters = false,
        includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
public class SpringMvcConfig implements WebMvcConfigurer {


    


    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

    @Bean
    public LocalValidatorFactoryBean localValidatorFactoryBean(){
        LocalValidatorFactoryBean localValidatorFactoryBean = new LocalValidatorFactoryBean();
        localValidatorFactoryBean.setProviderClass(HibernateValidator.class);
        return localValidatorFactoryBean;
    }

    @Bean
    public SpringResourceTemplateResolver springResourceTemplateResolver(){
        SpringResourceTemplateResolver springResourceTemplateResolver = new SpringResourceTemplateResolver();
        springResourceTemplateResolver.setPrefix("/WEB-INF/");
        springResourceTemplateResolver.setSuffix(".html");
        springResourceTemplateResolver.setCharacterEncoding("UTF-8");
        springResourceTemplateResolver.setOrder(1);
        springResourceTemplateResolver.setTemplateMode("HTML");
        springResourceTemplateResolver.setCacheable(false);
        return springResourceTemplateResolver;
    }


    @Bean
    public SpringTemplateEngine springTemplateEngine(SpringResourceTemplateResolver springResourceTemplateResolver){
        SpringTemplateEngine springTemplateEngine = new SpringTemplateEngine();
        springTemplateEngine.setTemplateResolver(springResourceTemplateResolver);
        return springTemplateEngine;
    }
    @Bean
    public ViewResolver viewResolver(SpringTemplateEngine springTemplateEngine){
        ThymeleafViewResolver thymeleafViewResolver = new ThymeleafViewResolver();
        thymeleafViewResolver.setCharacterEncoding("UTF-8");
        thymeleafViewResolver.setTemplateEngine(springTemplateEngine);
        return thymeleafViewResolver;
    }


}

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
    @PostMapping("/addStudent")
    public void handleFormSubmit(@Validated(ValidationGroup2.class)  Student student, BindingResult bindingResult){
           if (bindingResult != null){
            List<ObjectError> allErrors = bindingResult.getAllErrors();
            allErrors.forEach(error-> log.info("ObjectName => {},defaultMessage => {}",error.getObjectName(),error.getDefaultMessage()));
        }
    }
}
```