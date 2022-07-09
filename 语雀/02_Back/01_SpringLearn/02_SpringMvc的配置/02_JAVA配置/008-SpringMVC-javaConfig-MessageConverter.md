# SpringMVC-XmlConfig-MessageConverter

HttpMessageConverter接口：（有基于jackson,gson,fastJson的三个实现类）

> 负责把返回的对象转为JSON，把前端的JSON转为对象 ,此过程叫:序列化和反序列化的过程

# 1、MapperingJackson2HttpMessageConverter

SpringMVC已经提供基于jackson的这个实现类，开发者只需要把jackson依赖导入就可实现序列化和反序列化的事情。

- 加入依赖

```xml
   <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.13.2.2</version>
        </dependency>
```

- 得到JSON对象或者JSON数组

```java
@Controller
@RequestMapping("/book")
public class JsonController {

    @GetMapping("/getById")
    @ResponseBody
    public Book getObject(){
     return    Book.builder().id(1).name("JAVA精通大师").author("王乃醒").published(LocalDate.now()).build();
    }
    
    @ResponseBody
    @GetMapping("findAll")
    public List<Book> getAll(){
        ArrayList<Book> books = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
           books.add( Book.builder().id(1).name("JAVA精通大师"+i).author("王乃醒"+i).published(LocalDate.now()).build()) ;
        }
        return books;
    }
}

```

```json
{"id":1,"name":"JAVA精通大师","author":"王乃醒","published":"2022-03-06"}
```

```json
[{"id":1,"name":"JAVA精通大师0","author":"王乃醒0","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师1","author":"王乃醒1","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师2","author":"王乃醒2","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师3","author":"王乃醒3","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师4","author":"王乃醒4","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师5","author":"王乃醒5","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师6","author":"王乃醒6","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师7","author":"王乃醒7","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师8","author":"王乃醒8","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师9","author":"王乃醒9","published":"2022-03-06"}]
```

# 2、日期字段需要额外处理

## 2.1、Date字段转化处理

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Integer id;
    private String name;
    private String author;
    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")
    private Date published;
}

```

## 2.2、LocalDate And LocalDateTime

> Jaskson本来就LocalDateTime LocalDate 对于Java8的新日期APi并没有提供想要的序列化器和解析器，后优化跟上节奏才推出了，需要额外导包。

```xml
     <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.13.2.2</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.datatype</groupId>
            <artifactId>jackson-datatype-jsr310</artifactId>
            <version>2.13.2</version>
        </dependency>
```

- LocalDate

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Integer id;
    private String name;
    private String author;
    @JsonDeserialize(using = LocalDateDeserializer.class)
    @JsonSerialize(using = LocalDateSerializer.class)
    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")
    private LocalDate published;
}

```

- LocalDateTime

```java

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Integer id;
    private String name;
    private String author;
    @JsonDeserialize(using = LocalDateTimeDeserializer.class)
    @JsonSerialize(using = LocalDateTimeSerializer.class)
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone = "Asia/Shanghai")
    private LocalDateTime published;
}
```

# 3、日期全局配置

## 3.1、Date类型的全局配置

```java
@Configuration
@ComponentScan(basePackages = "com.wnx.springmvc",
        useDefaultFilters = false,
        includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
@EnableWebMvc
public class SpringMvcConfig implements WebMvcConfigurer {

    
    @Override
    public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        MappingJackson2HttpMessageConverter mappingJackson2HttpMessageConverter = new MappingJackson2HttpMessageConverter();
        ObjectMapper objectMapper = new ObjectMapper();
        //1.日期格式化和 时区格式化
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        objectMapper.setDateFormat(simpleDateFormat);
        objectMapper.setTimeZone(TimeZone.getTimeZone("GMT+8"));
        mappingJackson2HttpMessageConverter.setObjectMapper(objectMapper);

        //3.设置中文编码格式
        List<MediaType> list = new ArrayList<MediaType>();
        list.add(MediaType.APPLICATION_JSON);
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

> 好处：不用在POJO重复定义    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")

## 3.2、Date、LocalDate、LocalDateTime的全局配置

> 我的思路是：
>
> ObjectMapper,配置Date日期格式化的属性。
>
> SimpleModule加入LocalDateDeserializer LocalDateSerializer LocalDateTimeDeserializer LocalDateTimeSerializer 这四个解析器。
>
> Long类型在序列化的时候，使用ToStringSerializer，转为String.

```java
@Configuration
@ComponentScan(basePackages = "com.wnx.springmvc",
        useDefaultFilters = false,
        includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
@EnableWebMvc
public class SpringMvcConfig implements WebMvcConfigurer {

    @Override
    public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        MappingJackson2HttpMessageConverter mappingJackson2HttpMessageConverter = new MappingJackson2HttpMessageConverter();
        ObjectMapper objectMapper = new ObjectMapper();

        //1.日期格式化和 时区格式化
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        objectMapper.setDateFormat(simpleDateFormat);
        objectMapper.setTimeZone(TimeZone.getTimeZone(ZoneId.systemDefault());


        //3.设置中文编码格式
        List<MediaType> list = new ArrayList<MediaType>();
        list.add(MediaType.APPLICATION_JSON);


        //4.加入LocalDate LocalDateTime LocalTime序列化器和反序列化器
        SimpleModule simpleModule = new SimpleModule();
        simpleModule.addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
        simpleModule.addSerializer(LocalDate.class, new LocalDateSerializer(DateTimeFormatter.ofPattern("yyyy-MM-dd")));
        simpleModule.addSerializer(LocalTime.class, new LocalTimeSerializer(DateTimeFormatter.ofPattern("HH:mm:ss")));
        simpleModule.addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
        simpleModule.addDeserializer(LocalDate.class, new LocalDateDeserializer(DateTimeFormatter.ofPattern("yyyy-MM-dd")));
        simpleModule.addDeserializer(LocalTime.class, new LocalTimeDeserializer(DateTimeFormatter.ofPattern("HH:mm:ss")));

       //5.把Long类型序列化为String类型
        simpleModule.addSerializer(Long.class, ToStringSerializer.instance);

        objectMapper.registerModule(simpleModule);
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