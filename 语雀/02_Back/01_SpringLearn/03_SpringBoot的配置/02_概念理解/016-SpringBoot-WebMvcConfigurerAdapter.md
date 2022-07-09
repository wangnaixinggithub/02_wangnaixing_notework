# SpringBoot-WebMvcConfigurerAdapter 

# 1、WebMvcConfigurer

- `@EnableWebMvc`注解，开启`SpringMvc`的注解.

```java
public interface WebMvcConfigurer {
	//1.配置路径匹配
	default void configurePathMatch(PathMatchConfigurer configurer) {
	}
	//2.配置内容协商
	default void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
	}
	//3.配置异步支持
	default void configureAsyncSupport(AsyncSupportConfigurer configurer) {
	}
	//4.配置默认Servlet处理器处理静态资源
	default void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
	}
	//5.添加一些转换器或者格式化器
	default void addFormatters(FormatterRegistry registry) {
	}
	//6.添加拦截器
	default void addInterceptors(InterceptorRegistry registry) {
	}
	//4.添加静态资源处理器
	default void addResourceHandlers(ResourceHandlerRegistry registry) {
	}
	//5.添加跨域配置
	default void addCorsMappings(CorsRegistry registry) {
	}
	//6.添加纯视图无业务逻辑的控制器
	default void addViewControllers(ViewControllerRegistry registry) {
	}
	//7.配置自定义视图解析器
	default void configureViewResolvers(ViewResolverRegistry registry) {
	}
	//8.添加参数解析器
	default void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
	}
	//9.添加返回值处理器
	default void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> handlers) {
	}
	//10.配置消息转换器
	default void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
	}
	//11.扩展消息转换器
	default void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
	}
	//12.配置处理异常解析器
	default void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
	}
	//13.扩展异常处理解析器
	default void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
	}
	//14.校验器
	@Nullable
	default Validator getValidator() {
		return null;
	}
	//14.消息编码解析器
	default MessageCodesResolver getMessageCodesResolver() {
		return null;
	}

}
```

# 2、WebMvcConfigurationSupport

```java
public class WebMvcConfigurationSupport implements ApplicationContextAware, ServletContextAware {
  
    //1.应用上下文
    @Nullable
	private ApplicationContext applicationContext;
  
    //2.Servlet上下文
	@Nullable
	private ServletContext servletContext;

	@Override
	public void setApplicationContext(@Nullable ApplicationContext applicationContext) {
		this.applicationContext = applicationContext;
	}
    
	@Nullable
	public final ApplicationContext getApplicationContext() {
		return this.applicationContext;
	}
    
	@Override
	public void setServletContext(@Nullable ServletContext servletContext) {
		this.servletContext = servletContext;
	}
    
	@Nullable
	public final ServletContext getServletContext() {
		return this.servletContext;
	}
｝
```

```java
public class WebMvcConfigurationSupport implements ApplicationContextAware, ServletContextAware {
 
    //通过静态代码块，来通过反射，去根据类的全路径名创建一些Bean组件。
    private static final boolean romePresent;
	private static final boolean jaxb2Present;
	private static final boolean jackson2Present;
	private static final boolean jackson2XmlPresent;
	private static final boolean jackson2SmilePresent;
	private static final boolean jackson2CborPresent;
	private static final boolean gsonPresent;
	private static final boolean jsonbPresent;
	static {
        //1.得到WebMvc配置支持类的类加载器
		ClassLoader classLoader = WebMvcConfigurationSupport.class.getClassLoader();
      
        //2.通过反射工具创建当下实例并返回存在状态，赋值给WebMvc配置类的静态变量。
		romePresent = ClassUtils.isPresent("com.rometools.rome.feed.WireFeed", classLoader);
		
        jaxb2Present = ClassUtils.isPresent("javax.xml.bind.Binder", classLoader);
		
        jackson2Present = ClassUtils.isPresent("com.fasterxml.jackson.databind.ObjectMapper", classLoader) &			ClassUtils.isPresent("com.fasterxml.jackson.core.JsonGenerator", classLoader);	
        jackson2XmlPresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.xml.XmlMapper", classLoader);
		jackson2SmilePresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.smile.SmileFactory", classLoader);
		
        jackson2CborPresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.cbor.CBORFactory", classLoader);
		
        gsonPresent = ClassUtils.isPresent("com.google.gson.Gson", classLoader);
		jsonbPresent =  ClassUtils.isPresent("javax.json.bind.Jsonb", classLoader);
	}
｝
```

```java
public abstract class ClassUtils {
			public static boolean isPresent(String className, @Nullable ClassLoader classLoader) {
		try {
			forName(className, classLoader);
			return true;
		}
		catch (IllegalAccessError err) {
			throw new IllegalStateException("Readability mismatch in inheritance hierarchy of class [" +
					className + "]: " + err.getMessage(), err);
		}
		catch (Throwable ex) {
			return false;
		}
	}
｝-
```

```java
public abstract class ClassUtils {
    @Nullable
	private List<Object> interceptors;
	@Nullable
	private PathMatchConfigurer pathMatchConfigurer;
	@Nullable
	private ContentNegotiationManager contentNegotiationManager;
	@Nullable
	private List<HandlerMethodArgumentResolver> argumentResolvers;
	@Nullable
	private List<HandlerMethodReturnValueHandler> returnValueHandlers;
	@Nullable
	private List<HttpMessageConverter<?>> messageConverters;
	@Nullable
	private Map<String, CorsConfiguration> corsConfigurations;
	
｝
```

# 3、RequestMappingHandlerMapping

```java
public class WebMvcConfigurationSupport implements ApplicationContextAware, ServletContextAware {
    
    //装配RequestMapperingHandler
    @Bean
	@SuppressWarnings("deprecation")
	public RequestMappingHandlerMapping requestMappingHandlerMapping(
			@Qualifier("mvcContentNegotiationManager") ContentNegotiationManager contentNegotiationManager,
			@Qualifier("mvcConversionService") FormattingConversionService conversionService,
			@Qualifier("mvcResourceUrlProvider") ResourceUrlProvider resourceUrlProvider) {
        //1.创建一个请求处理器映射器，对他的成员变量进行一顿初始化，如拦截器，内容协商管理器，跨域配置。
		RequestMappingHandlerMapping mapping = createRequestMappingHandlerMapping();
		mapping.setOrder(0);
		mapping.setInterceptors(getInterceptors(conversionService, resourceUrlProvider));
		mapping.setContentNegotiationManager(contentNegotiationManager);
		mapping.setCorsConfigurations(getCorsConfigurations());
       
        //2.得到路径匹配器，进行路径匹配的判断
		PathMatchConfigurer configurer = getPathMatchConfigurer();
		Boolean useSuffixPatternMatch = configurer.isUseSuffixPatternMatch();
		if (useSuffixPatternMatch != null) {
			mapping.setUseSuffixPatternMatch(useSuffixPatternMatch);
		}
		Boolean useRegisteredSuffixPatternMatch = configurer.isUseRegisteredSuffixPatternMatch();
		
        if (useRegisteredSuffixPatternMatch != null) {
			mapping.setUseRegisteredSuffixPatternMatch(useRegisteredSuffixPatternMatch);
		}
		
        Boolean useTrailingSlashMatch = configurer.isUseTrailingSlashMatch();
		if (useTrailingSlashMatch != null) {
			mapping.setUseTrailingSlashMatch(useTrailingSlashMatch);
		}
		
        UrlPathHelper pathHelper = configurer.getUrlPathHelper();
		if (pathHelper != null) {
			mapping.setUrlPathHelper(pathHelper);
		}
		
        PathMatcher pathMatcher = configurer.getPathMatcher();
		if (pathMatcher != null) {
			mapping.setPathMatcher(pathMatcher);
		}
		
        Map<String, Predicate<Class<?>>> pathPrefixes = configurer.getPathPrefixes();
		if (pathPrefixes != null) {
			mapping.setPathPrefixes(pathPrefixes);
		}
		return mapping;
	}


	protected RequestMappingHandlerMapping createRequestMappingHandlerMapping() {
		return new RequestMappingHandlerMapping();
	}

	
  
}
```

# 4、BeanNameUrlHandlerMappering

```java
public class WebMvcConfigurationSupport implements ApplicationContextAware, ServletContextAware {
    //装配BeanNameUrlHandlerMappering
        @Bean
        public BeanNameUrlHandlerMapping beanNameHandlerMapping(
                @Qualifier("mvcConversionService") FormattingConversionService conversionService,
                @Qualifier("mvcResourceUrlProvider") ResourceUrlProvider resourceUrlProvider) {

            BeanNameUrlHandlerMapping mapping = new BeanNameUrlHandlerMapping();
            mapping.setOrder(2);
            mapping.setInterceptors(getInterceptors(conversionService, resourceUrlProvider));
            mapping.setCorsConfigurations(getCorsConfigurations());
            return mapping;
        }
  }
```



# 5、RequestMappingHandlerAdapter

```java
   public class WebMvcConfigurationSupport implements ApplicationContextAware, ServletContextAware {	
    @Bean
	public RequestMappingHandlerAdapter requestMappingHandlerAdapter(
			@Qualifier("mvcContentNegotiationManager") ContentNegotiationManager contentNegotiationManager,
			@Qualifier("mvcConversionService") FormattingConversionService conversionService,
			@Qualifier("mvcValidator") Validator validator) {
		//1.创建请求映射处理器适配器
		RequestMappingHandlerAdapter adapter = createRequestMappingHandlerAdapter();
        //2.设置内容协商管理器
		adapter.setContentNegotiationManager(contentNegotiationManager);
        //3.设置消息转换器
		adapter.setMessageConverters(getMessageConverters());
       //4.设置初始标注有@WebInit的类的初始化器
		adapter.setWebBindingInitializer(getConfigurableWebBindingInitializer(conversionService, validator));
        //5.设置方法参数解析器
		adapter.setCustomArgumentResolvers(getArgumentResolvers());
        //6.设置返回值处理器
		adapter.setCustomReturnValueHandlers(getReturnValueHandlers());
		//7.如果有json解析包存在，就开启jSON响应体模式。 
		if (jackson2Present) {
			adapter.setRequestBodyAdvice(Collections.singletonList(new JsonViewRequestBodyAdvice()));
			adapter.setResponseBodyAdvice(Collections.singletonList(new JsonViewResponseBodyAdvice()));
		}
        //6.异步支持配置
		AsyncSupportConfigurer configurer = new AsyncSupportConfigurer();
		configureAsyncSupport(configurer);
		if (configurer.getTaskExecutor() != null) {
			adapter.setTaskExecutor(configurer.getTaskExecutor());
		}
		if (configurer.getTimeout() != null) {
			adapter.setAsyncRequestTimeout(configurer.getTimeout());
		}
        //7.一些拦截器
		adapter.setCallableInterceptors(configurer.getCallableInterceptors());
		adapter.setDeferredResultInterceptors(configurer.getDeferredResultInterceptors());

		return adapter;
	}
	protected RequestMappingHandlerAdapter createRequestMappingHandlerAdapter() {
		return new RequestMappingHandlerAdapter();
	}
  	//HTTP处理器适配器     
    @Bean
    public HttpRequestHandlerAdapter httpRequestHandlerAdapter() {
       return new HttpRequestHandlerAdapter();
    }
       
 }
```

```java
public class HttpRequestHandlerAdapter implements HandlerAdapter {
	//#supports()
    @Override
	public boolean supports(Object handler) {
		return (handler instanceof HttpRequestHandler); //处理Http请求处理器
	}
    //#handle()
	@Override
	@Nullable
	public ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
         //拿到请求处理器，进行强制类型转换，调用它的handlerRequest方法。
		((HttpRequestHandler) handler).handleRequest(request, response);
		return null;
	}
    //#getLastModified()
	@Override
	public long getLastModified(HttpServletRequest request, Object handler) {
		if (handler instanceof LastModified) {
			return ((LastModified) handler).getLastModified(request);
		}
		return -1L;
	}
    
}
```

```java
@FunctionalInterface
public interface HttpRequestHandler {
    void handleRequest(HttpServletRequest var1, HttpServletResponse var2) throws ServletException, IOException;
}
```

# 6、SimpleControllerHandlerAdapter

```java
public class WebMvcConfigurationSupport implements ApplicationContextAware, ServletContextAware {	
	@Bean
	public SimpleControllerHandlerAdapter simpleControllerHandlerAdapter() {
		return new SimpleControllerHandlerAdapter();
	}
}
```

```java
public class SimpleControllerHandlerAdapter implements HandlerAdapter {
	//#supports()
    @Override
	public boolean supports(Object handler) {
		return (handler instanceof Controller);
	}
    
    //#handle()
	@Override
	@Nullable
	public ModelAndView handle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {

		return ((Controller) handler).handleRequest(request, response);
	}
    
    //#getLastModified()
	@Override
	public long getLastModified(HttpServletRequest request, Object handler) {
		if (handler instanceof LastModified) {
			return ((LastModified) handler).getLastModified(request);
		}
		return -1L;
	}
}
```

```java
@FunctionalInterface
public interface Controller {
	@Nullable
	ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception;

}
```

# 7、@EnableWebMvc

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Import(DelegatingWebMvcConfiguration.class) //1,注入了代理的WebMvc配置类:DelegatingWebMvcConfiguration
public @interface EnableWebMvc {
}
```

```java
//该配置类实现了webMvc配置支持类
@Configuration(proxyBeanMethods = false)
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {

	private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
｝
```

