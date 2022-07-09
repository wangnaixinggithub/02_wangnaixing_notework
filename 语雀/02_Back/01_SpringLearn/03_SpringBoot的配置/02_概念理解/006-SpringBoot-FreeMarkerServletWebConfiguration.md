# SpringBoot-FreeMarkerServletWebConfiguration

```xml
      <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-freemarker</artifactId>
        </dependency>
```

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass({ freemarker.template.Configuration.class, FreeMarkerConfigurationFactory.class })
@EnableConfigurationProperties(FreeMarkerProperties.class)
@Import({ FreeMarkerServletWebConfiguration.class, FreeMarkerReactiveWebConfiguration.class,
		FreeMarkerNonWebConfiguration.class })
public class FreeMarkerAutoConfiguration {
}
```

```java
@ConfigurationProperties(prefix = "spring.freemarker")
public class FreeMarkerProperties extends AbstractTemplateViewResolverProperties {
	public static final String DEFAULT_TEMPLATE_LOADER_PATH = "classpath:/templates/";
	public static final String DEFAULT_PREFIX = "";
	public static final String DEFAULT_SUFFIX = ".ftlh";
    //...
}
```

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)
@ConditionalOnClass({ Servlet.class, FreeMarkerConfigurer.class })
@AutoConfigureAfter(WebMvcAutoConfiguration.class)
class FreeMarkerServletWebConfiguration extends AbstractFreeMarkerConfiguration {

}
```

