# SpringBoot-No-Statis-Intercepter

# 1、告别配置

```xml
    <mvc:resources mapping="/js/**" location="/static/js/"/>
    <mvc:resources mapping="/css/**" location="/static/css/"/>
    <mvc:resources mapping="/images/**" location="/static/images/"/>
```

```xml
    <mvc:resources mapping="/**" location="/static/"/>
```

# 2、约定先行

> 约定静态资源都放在：resouces/static目录下

我们启动SpringBoot工程，不再需要配置直接可以访问到

```html
http://localhost:8080/js/jquery-3.1.1.js
http://localhost:8080/css/index.css
http://localhost:8080/images/background.png
```

# 3、WebMvcAutoConfiguration

```java
        @Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
			addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
			addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
				registration.addResourceLocations(this.resourceProperties.getStaticLocations());
				if (this.servletContext != null) {
					ServletContextResource resource = new ServletContextResource(this.servletContext, SERVLET_LOCATION);
					registration.addResourceLocations(resource);
				}
			});
		}

		private void addResourceHandler(ResourceHandlerRegistry registry, String pattern, String... locations) {
			addResourceHandler(registry, pattern, (registration) -> registration.addResourceLocations(locations));
		}
```

![](007-SpringBoot-No-Statis-Intercepter.assets/image-20220327160451596.png)

# 4、WebMvcConfig

```java
//自定义覆盖静态资源规则
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/**")
                .addResourceLocations("classpath:/wnx/")
                .addResourceLocations("classpath:/static/");
    }
}
```

