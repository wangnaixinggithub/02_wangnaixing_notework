

# SpringMvc-XmlConfig-CustomerView-BeanNameViewResolver

## 1、BeanNameViewResolver处理beanName视图

```java
@Configuration
@ComponentScan(basePackages = "com.wnx.springmvc",
        useDefaultFilters = false,
        includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
@EnableWebMvc
public class SpringMvcConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        MyInterceptor1 myInterceptor1 = new MyInterceptor1();
        MyInterceptor2 myInterceptor2 = new MyInterceptor2();
        registry.addInterceptor(myInterceptor1).addPathPatterns("/**");
        registry.addInterceptor(myInterceptor2).addPathPatterns("/**");
    }

    @Override
    public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        MappingJackson2HttpMessageConverter mappingJackson2HttpMessageConverter = new MappingJackson2HttpMessageConverter();
        ObjectMapper objectMapper = new ObjectMapper();

        //1.日期格式化和 时区格式化
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        objectMapper.setDateFormat(simpleDateFormat);
        objectMapper.setLocale(Locale.getDefault());
        objectMapper.setTimeZone(TimeZone.getTimeZone(ZoneId.systemDefault()));


        //3.设置中文编码格式
        List<MediaType> list = new ArrayList<MediaType>();
        list.add(MediaType.APPLICATION_JSON);


        //4.加入LocalDate LocalDateTime LocalTime序列化器和反序列化器
        SimpleModule simpleModule = new SimpleModule();


       //5.把Long类型序列化为String类型
        simpleModule.addSerializer(Long.class, ToStringSerializer.instance);

        objectMapper.registerModules(simpleModule,new JavaTimeModule());
        mappingJackson2HttpMessageConverter.setObjectMapper(objectMapper);
        mappingJackson2HttpMessageConverter.setSupportedMediaTypes(list);

        converters.add(0, mappingJackson2HttpMessageConverter);
    }

     //使用默认的Servlet处理静态资源
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
    public ViewResolver viewResolver(){
        return new BeanNameViewResolver();

    }

}
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

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/060-%E9%9B%B7%E4%B8%B0%E9%98%B3-SpringMVC%E8%A7%86%E5%9B%BE%E8%A7%A3%E6%9E%90%E5%99%A8.assets/image-20220509230800714.png)

