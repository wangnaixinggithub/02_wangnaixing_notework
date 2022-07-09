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



## 2、SimpleUrlHandlerMapping And HttpRequestHandlerAdapter

> 处理器映射器：使用beanName和Url维护一个prop的处理器映射器，SimpleUrlHandlerMapping.
>
> 处理器适配器：处理实现HttpRequestHandler接口的处理适配器，HttpRequestHandlerAdapter

```java
@Configuration
@ComponentScan(basePackages = "com.wnx.springmvc",
        useDefaultFilters = false,
        includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
public class SpringMvcConfig implements WebMvcConfigurer {

    

    @Bean
    public HandlerMapping beanNameUrlHandlerMapping(){
        SimpleUrlHandlerMapping simpleUrlHandlerMapping = new SimpleUrlHandlerMapping();
        Properties prop = new Properties();
        prop.setProperty("/addUser","userController");
        simpleUrlHandlerMapping.setMappings(prop);
        return simpleUrlHandlerMapping;
    }
    

    @Bean
    public HandlerAdapter handlerAdapter(){
        return new HttpRequestHandlerAdapter();

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