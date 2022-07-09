# SpringBoot-ThymeleafAutoConfiguration

```xml
     <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
     </dependency>
```

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {
	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;
	public static final String DEFAULT_PREFIX = "classpath:/templates/";
	public static final String DEFAULT_SUFFIX = ".html";
	private boolean checkTemplate = true;
	private boolean checkTemplateLocation = true;
	private String prefix = DEFAULT_PREFIX;
	private String suffix = DEFAULT_SUFFIX;
	private String mode = "HTML";
	private Charset encoding = DEFAULT_ENCODING;
	private boolean cache = true;
	private Integer templateResolverOrder;
	private String[] viewNames;
	private String[] excludedViewNames;
	private boolean enableSpringElCompiler;
	private boolean renderHiddenMarkersBeforeCheckboxes = false;
	private boolean enabled = true;
    //...
}
```

```java
@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(ThymeleafProperties.class)
@ConditionalOnClass({ TemplateMode.class, SpringTemplateEngine.class }) //必须要由TemplateMode类，和SpringTemplateEngine类才存在这个配置
@AutoConfigureAfter({ WebMvcAutoConfiguration.class, WebFluxAutoConfiguration.class })
public class ThymeleafAutoConfiguration {

```

