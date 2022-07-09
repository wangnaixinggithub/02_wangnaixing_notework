# SimpleUrlHandlerMapping And SimpleControllerHandlerAdapter

## 1、SimpleUrlHandlerMapping维护properties对象，可配置多个URL和handler的映射关系

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
        return new SimpleControllerHandlerAdapter();

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

## 2、业务逻辑

```java
@Log4j2
@Controller
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