# SSM

# 1、WebXmlConfig

```java
package com.wnx.springmvc.config;

import org.springframework.web.filter.CharacterEncodingFilter;
import org.springframework.web.filter.HiddenHttpMethodFilter;
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

import javax.servlet.Filter;

public class WebXmlConfig extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        characterEncodingFilter.setEncoding("UTF-8");
        characterEncodingFilter.setForceRequestEncoding(true);
        characterEncodingFilter.setForceResponseEncoding(true);


        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();

        return new Filter[] {characterEncodingFilter,hiddenHttpMethodFilter};
    }
}

```

# 2、SpringMvcConfig

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
    //配置通用Multipart解析器
    @SneakyThrows
    @Bean
    public MultipartResolver commonMultipartResolver(){
        CommonsMultipartResolver commonsMultipartResolver = new CommonsMultipartResolver();
        commonsMultipartResolver.setDefaultEncoding("UTF-8");
        commonsMultipartResolver.setMaxUploadSize(1024000L);
        commonsMultipartResolver.setMaxUploadSize(1024000L);
        commonsMultipartResolver.setUploadTempDir(new FileSystemResource("/WEB-INF/temp"));
        commonsMultipartResolver.setMaxInMemorySize(102400);
        return commonsMultipartResolver;
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

# 3、SpringConfig

```java
package com.wnx.springmvc.config;

import com.alibaba.druid.pool.DruidDataSource;
import com.wnx.springmvc.model.Dept;
import com.wnx.springmvc.model.Emp;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.*;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;

@Configuration
@ComponentScan(
        basePackages = "com.wnx.springmvc",
        excludeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,classes = Controller.class)})
@PropertySource("classpath:/jdbc.properties")
@EnableAspectJAutoProxy      
@EnableTransactionManagement
public class SpringConfig {
    
    @Bean
    public DataSource dataSource(@Value("jdbc.username") String username,
                                 @Value("jdbc.password") String password,
                                 @Value("jdbc.driverClassName")String driverClassName,
                                 @Value("url") String url){
          DruidDataSource dataSource = new DruidDataSource();
          dataSource.setUsername(username);
          dataSource.setPassword(password);
          dataSource.setDriverClassName(driverClassName);
          dataSource.setUrl(url);
          return dataSource;


    }
    
     @Bean
    public TransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
        mapperScannerConfigurer.setBasePackage("com.wnx.springmvc.mapper");
        return mapperScannerConfigurer;
    }
    

    @Bean
    public MybatisSqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource, GlobalConfig globalConfig, MybatisPlusInterceptor mybatisPlusInterceptor){
        MybatisSqlSessionFactoryBean mybatisSqlSessionFactoryBean = new MybatisSqlSessionFactoryBean();
        mybatisSqlSessionFactoryBean.setDataSource(dataSource);
        mybatisSqlSessionFactoryBean.setTypeHandlersPackage("com.wnx.springmvc.model");
        mybatisSqlSessionFactoryBean.setTypeEnumsPackage("com.wnx.springmvc.enumerate");
        mybatisSqlSessionFactoryBean.setGlobalConfig(globalConfig);
        mybatisSqlSessionFactoryBean.setPlugins(mybatisPlusInterceptor);
        return mybatisSqlSessionFactoryBean;
    }
    
    @Bean
    public GlobalConfig globalConfig(){
        GlobalConfig globalConfig = new GlobalConfig();
        GlobalConfig.DbConfig dbConfig = new GlobalConfig.DbConfig();
        dbConfig.setIdType(IdType.AUTO);
        dbConfig.setTablePrefix("t_");
        globalConfig.setDbConfig(dbConfig);
        return globalConfig;
    }
    
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor();
        OptimisticLockerInnerInterceptor optimisticLockerInnerInterceptor = new OptimisticLockerInnerInterceptor();
        paginationInnerInterceptor.setDbType(DbType.MYSQL);
        List<InnerInterceptor> list = new ArrayList<>();
        list.add(paginationInnerInterceptor);
        list.add(optimisticLockerInnerInterceptor);
        mybatisPlusInterceptor.setInterceptors(list);
        return mybatisPlusInterceptor;
    }



    

    

}

```



