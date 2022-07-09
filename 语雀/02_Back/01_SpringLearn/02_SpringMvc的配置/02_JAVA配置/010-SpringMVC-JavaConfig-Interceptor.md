# SpringMVC-JavaConfig-Interceptor

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

```java
@Configuration
@ComponentScan(basePackages = "com.wnx.springmvc",
        useDefaultFilters = false,
        includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
@EnableWebMvc
public class SpringMvcConfig implements WebMvcConfigurer {

	//添加拦截器
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

 
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        objectMapper.setDateFormat(simpleDateFormat);
        objectMapper.setLocale(Locale.getDefault());
        objectMapper.setTimeZone(TimeZone.getTimeZone(ZoneId.systemDefault()));


        
        List<MediaType> list = new ArrayList<MediaType>();
        list.add(MediaType.APPLICATION_JSON);


 
        SimpleModule simpleModule = new SimpleModule();



        simpleModule.addSerializer(Long.class, ToStringSerializer.instance);

        objectMapper.registerModules(simpleModule,new JavaTimeModule());
        mappingJackson2HttpMessageConverter.setObjectMapper(objectMapper);
        mappingJackson2HttpMessageConverter.setSupportedMediaTypes(list);
        
        converters.add(0, mappingJackson2HttpMessageConverter);
    }
    

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

## 2、两个拦截器的执行流程

> 配置中，myInterceptor1在myInterceptor2之前，所以先走myInterceptor1.

```c#
com.wnx.spring.filter.FirstHandlerInterceptorpreHandle..............
com.wnx.spring.filter.SecondHandlerInterceptorpreHandle..............
Handler方法执行了哦..............
com.wnx.spring.filter.SecondHandlerInterceptorpostHandle..............
com.wnx.spring.filter.FirstHandlerInterceptorpostHandle..............
com.wnx.spring.filter.SecondHandlerInterceptorafterCompletion..............
com.wnx.spring.filter.FirstHandlerInterceptorafterCompletion..............
```