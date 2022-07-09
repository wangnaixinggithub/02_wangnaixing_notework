# SpringMvc-RequestMappingHandlerMapping-RequestMappingHandlerAdapter

## 1、使用@RequestMapping注解，来标注该组件为handler.

```java
@Log4j2
@Controller
public class UserController {

    private final UserService userService;
    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }


    @RequestMapping("/addUser")
    public ModelAndView addUser() throws Exception {
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

```

```html
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

## 2、配置RequestMappingHandlerMapping-RequestMappingHandlerAdapter

> 处理器映射器：处理标注了@RequestMapping的handler之间URL和handler的映射关系
>
> 处理器适配器：处理标注了@RequestMapping的handler的业务执行

```java
package com.wnx.springmvc.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.stereotype.Controller;
import org.springframework.web.servlet.HandlerAdapter;
import org.springframework.web.servlet.HandlerMapping;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter;
import org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping;
import org.thymeleaf.spring5.SpringTemplateEngine;
import org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver;
import org.thymeleaf.spring5.view.ThymeleafViewResolver;

@Configuration
@ComponentScan(basePackages = "com.wnx.springmvc",
        useDefaultFilters = false,
        includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
public class SpringMvcConfig implements WebMvcConfigurer {


    @Bean
    public HandlerMapping requestMappingHandlerMapping(){
        return new RequestMappingHandlerMapping();
    }

    @Bean
    public HandlerAdapter handlerAdapter(){
        return new RequestMappingHandlerAdapter();
    }

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
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