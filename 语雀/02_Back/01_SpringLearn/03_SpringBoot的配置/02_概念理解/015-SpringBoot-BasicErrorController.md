# SpringBoot-BasicErrorController

> SpringBoot中当出现异常时，会到template/error目录下找到以错误编码命名的错误动态视图，如果找不到会到static/error目录下找以错误编码命名的错误静态视图，还是找不到那就向浏览器回滚找到页面。

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/123-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-SpringBoot%E4%B8%AD%E8%87%AA%E5%AE%9A%E4%B9%89%E5%BC%82%E5%B8%B8%E8%A7%86%E5%9B%BE.assets/image-20220327204404498.png)

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/123-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-SpringBoot%E4%B8%AD%E8%87%AA%E5%AE%9A%E4%B9%89%E5%BC%82%E5%B8%B8%E8%A7%86%E5%9B%BE.assets/image-20220327204413938.png)

> 定制错误页面位置

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/123-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-SpringBoot%E4%B8%AD%E8%87%AA%E5%AE%9A%E4%B9%89%E5%BC%82%E5%B8%B8%E8%A7%86%E5%9B%BE.assets/image-20220327204744808.png)

# 1、BasicErrorController

```java
    //  1.YAML中找server.error.path属性配置有吗？有就优先用 
    // 2.${error.path}配置有吗？有就优先用
    // 3./error
@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {
	
    //<1>处理错误响应视图的(HTML)
    @RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
	public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
		//1.获取请求HttpStatus
        HttpStatus status = getStatus(request);
		
        //2.获取请求所有的错误属性
        Map<String, Object> model = Collections
				.unmodifiableMap(getErrorAttributes(request, getErrorAttributeOptions(request, MediaType.TEXT_HTML)));
        
        //3.解析得到错误视图
        response.setStatus(status.value());
		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
		
       //4.创建名称为error的ModelAndView
        return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);
	}
    
    //2.处理返回响应体的（JSON）
    @RequestMapping
	public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
		HttpStatus status = getStatus(request);
		if (status == HttpStatus.NO_CONTENT) {
			return new ResponseEntity<>(status);
		}
        //1.同样获取所有的错误属性
		Map<String, Object> body = getErrorAttributes(request, getErrorAttributeOptions(request, MediaType.ALL));
        
        //2.封装成响应实体
		return new ResponseEntity<>(body, status);
	}
}
```

# 2、ErrorTemplateMissingCondition

```java
//未检测到错误模板视图时匹配的SpringBootCondition 。
private static class ErrorTemplateMissingCondition extends SpringBootCondition {
		
    @Override
		public ConditionOutcome getMatchOutcome(ConditionContext context, AnnotatedTypeMetadata metadata) {
			ConditionMessage.Builder message = ConditionMessage.forCondition("ErrorTemplate Missing");
			
            TemplateAvailabilityProviders providers = new TemplateAvailabilityProviders(context.getClassLoader());
            
			TemplateAvailabilityProvider provider = providers.getProvider("error", context.getEnvironment(),
					context.getClassLoader(), context.getResourceLoader());
			
            if (provider != null) {
				return ConditionOutcome.noMatch(message.foundExactly("template from " + provider));
			}
			
            return ConditionOutcome.match(message.didNotFind("error template view").atAll());
		}

	}
```

# 3、WhitelabelErrorViewConfiguration

```java
	@Configuration(proxyBeanMethods = false)
	@ConditionalOnProperty(prefix = "server.error.whitelabel", name = "enabled", matchIfMissing = true)
	@Conditional(ErrorTemplateMissingCondition.class)
	protected static class WhitelabelErrorViewConfiguration {
        //直接new了一个！
		private final StaticView defaultErrorView = new StaticView();
		
        @Bean(name = "error")
		@ConditionalOnMissingBean(name = "error")
		public View defaultErrorView() {
			return this.defaultErrorView;
		}
        
        @Bean
		@ConditionalOnMissingBean
		public BeanNameViewResolver beanNameViewResolver() {
			BeanNameViewResolver resolver = new BeanNameViewResolver();
			resolver.setOrder(Ordered.LOWEST_PRECEDENCE - 10);
			return resolver;
		}
	}
```

# 4、StaticView

```java
private static class StaticView implements View {

		private static final MediaType TEXT_HTML_UTF8 = new MediaType("text", "html", StandardCharsets.UTF_8);
		private static final Log logger = LogFactory.getLog(StaticView.class);
		
		@Override
		public void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse response)
				throws Exception {
			if (response.isCommitted()) {
				String message = getMessage(model);
				logger.error(message);
				return;
			}
            
			response.setContentType(TEXT_HTML_UTF8.toString());
			StringBuilder builder = new StringBuilder();
			Object timestamp = model.get("timestamp");
			Object message = model.get("message");
			Object trace = model.get("trace");
            
			if (response.getContentType() == null) {
				response.setContentType(getContentType());
			}
            
			builder.append("<html><body><h1>Whitelabel Error Page</h1>").append(
					"<p>This application has no explicit mapping for /error, so you are seeing this as a fallback.</p>")
					.append("<div id='created'>").append(timestamp).append("</div>")
					.append("<div>There was an unexpected error (type=").append(htmlEscape(model.get("error")))
					.append(", status=").append(htmlEscape(model.get("status"))).append(").</div>");
			if (message != null) {
				builder.append("<div>").append(htmlEscape(message)).append("</div>");
			}
			if (trace != null) {
				builder.append("<div style='white-space:pre-wrap;'>").append(htmlEscape(trace)).append("</div>");
			}
			builder.append("</body></html>");
			response.getWriter().append(builder.toString());
		}
        

		@Override
		public String getContentType() {
			return "text/html";
		}
    
	}
```

# 5、AbstractErrorController

```java
public abstract class AbstractErrorController implements ErrorController {
		private final ErrorAttributes errorAttributes;
		private final List<ErrorViewResolver> errorViewResolvers;
		
		protected Map<String, Object> getErrorAttributes(HttpServletRequest request, ErrorAttributeOptions options) {
		WebRequest webRequest = new ServletWebRequest(request);
		return this.errorAttributes.getErrorAttributes(webRequest, options);
	}
	
		protected ModelAndView resolveErrorView(HttpServletRequest request, HttpServletResponse response, HttpStatus status,
			Map<String, Object> model) {
		for (ErrorViewResolver resolver : this.errorViewResolvers) {
			ModelAndView modelAndView = resolver.resolveErrorView(request, status, model);
			if (modelAndView != null) {
				return modelAndView;
			}
		}
		return null;
	}
}
```

# 6、DefaultErrorAttributes

```java
public class DefaultErrorAttributes{
    	//DefaultErrorAttributes 为 getErrorAttributes() 的最终实现。
		private Map<String, Object> getErrorAttributes(WebRequest webRequest, boolean includeStackTrace) {
		Map<String, Object> errorAttributes = new LinkedHashMap<>();
		
        errorAttributes.put("timestamp", new Date());
        addStatus(errorAttributes, webRequest);
		addErrorDetails(errorAttributes, webRequest, includeStackTrace);
		addPath(errorAttributes, webRequest);
		
        return errorAttributes;
	}
}
```

# 7、ErrorMvcAutoConfiguration

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class })
@AutoConfigureBefore(WebMvcAutoConfiguration.class)
@EnableConfigurationProperties({ ServerProperties.class, WebMvcProperties.class })
public class ErrorMvcAutoConfiguration { 
	
    //装配 DefaultErrorAttributes 组件
    @Bean
	@ConditionalOnMissingBean(value = ErrorAttributes.class, search = SearchStrategy.CURRENT)
	public DefaultErrorAttributes errorAttributes() {
		return new DefaultErrorAttributes();
	}
    
    
    
    @Configuration(proxyBeanMethods = false)
	@EnableConfigurationProperties({WebProperties.class, WebMvcProperties.class })
	static class DefaultErrorViewResolverConfiguration {

		private final ApplicationContext applicationContext;

		private final Resources resources;

		DefaultErrorViewResolverConfiguration(ApplicationContext applicationContext, WebProperties webProperties) {
			this.applicationContext = applicationContext;
			this.resources = webProperties.getResources();
		}

       	// 装配默认错误视图解析器
		@Bean
		@ConditionalOnBean(DispatcherServlet.class)
		@ConditionalOnMissingBean(ErrorViewResolver.class)
		DefaultErrorViewResolver conventionErrorViewResolver() {
			return new DefaultErrorViewResolver(this.applicationContext, this.resources);
		}

}

```

# 8、DefaultErrorViewResolver

<img src="123-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-SpringBoot%E4%B8%AD%E8%87%AA%E5%AE%9A%E4%B9%89%E5%BC%82%E5%B8%B8%E8%A7%86%E5%9B%BE.assets/image-20220327215027555.png" alt="image-20220327215027555"  />

```java
public class DefaultErrorViewResolver implements ErrorViewResolver, Ordered { 
    //1.解析错误视图的方法
    @Override
	public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
        //1.调用解析resolve() 得到ModelAndView
		ModelAndView modelAndView = resolve(String.valueOf(status.value()), model);
		
        if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
			modelAndView = resolve(SERIES_VIEWS.get(status.series()), model);
		}
        
        //2.并返回。
		return modelAndView;
	}

	private ModelAndView resolve(String viewName, Map<String, Object> model) {
		String errorViewName = "error/" + viewName;
        //1.注入模板的可用性提供者
        TemplateAvailabilityProvider provider = this.templateAvailabilityProviders.getProvider(errorViewName,
				this.applicationContext);
	
        if (provider != null) {
			return new ModelAndView(errorViewName, model);
		}
        
        //2.解析资源
		return resolveResource(errorViewName, model);
	}
    
	private ModelAndView resolveResource(String viewName, Map<String, Object> model) {
		for (String location : this.resources.getStaticLocations()) {
			
            try {
                //1.获取Resource
				Resource resource = this.applicationContext.getResource(location);
				resource = resource.createRelative(viewName + ".html");
				if (resource.exists()) {
                    //2.总之,包装HTML。
					return new ModelAndView(new HtmlResourceView(resource), model);
				}
			}
			catch (Exception ex) {
			}
		}
        
        
	private static class HtmlResourceView implements View {
		private Resource resource;
		HtmlResourceView(Resource resource) {
			this.resource = resource;
		}
		@Override
		public String getContentType() {
			return MediaType.TEXT_HTML_VALUE;
		}
		@Override
		public void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse response)
				throws Exception {
			response.setContentType(getContentType());
			FileCopyUtils.copy(this.resource.getInputStream(), response.getOutputStream());
		}
	}
        
  }      
```

# 9、MyErrorViewResolver

```java
@Component
public class MyErrorViewResolver implements ErrorViewResolver {
    @Override
    public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
        return new ModelAndView("/wangnaixing/wnx",model);
    }
}
```

- 好家伙，这个页面可是在templates页面下wangnaixing文件夹下的wnx.html哦！

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>异常也买你</title>
</head>
<body>
<h1>系统出现了异常哦！</h1>
<table>
    <tr>
        <td>请求路径</td>
        <td th:text="${path}"></td>
    </tr>
    <tr>
        <td>请求错误时间</td>
        <td th:text="${timestamp}"></td>
    </tr>
    <tr>
        <td>请求状态码</td>
        <td th:text="${status}"></td>
    </tr>
    <tr>
        <td>错误信息</td>
        <td th:text="${error}"></td>
    </tr>
</table>
</body>
</html>
```

