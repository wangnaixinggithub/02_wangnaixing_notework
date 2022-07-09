# Spring-JavaConfig-JPA

```xml
     <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.6.8.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate.javax.persistence</groupId>
            <artifactId>hibernate-jpa-2.1-api</artifactId>
            <version>1.0.2.Final</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-jpa</artifactId>
            <version>2.6.4</version>
        </dependency>
```

```java
@Configuration
@PropertySource("classpath:/jdbc.properties")
@EnableTransactionManagement
@EnableJpaRepositories(
        basePackages = "com.wnx.spring.repository",
        entityManagerFactoryRef = "entityManagerFactoryBean"
)
public class SpringConfig {
     @Bean
    public DataSource dataSource(
            @Value("${jdbc.username}") String username,
            @Value("${jdbc.password}") String password,
            @Value("${jdbc.driverClassName}") String driverClassName,
            @Value("${jdbc.url}") String url

    ){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        dataSource.setUrl(url);
        dataSource.setDriverClassName(driverClassName);

        return dataSource;
    }
   @Bean
   public LocalContainerEntityManagerFactoryBean entityManagerFactoryBean(DataSource dataSource){
       LocalContainerEntityManagerFactoryBean localContainerEntityManagerFactoryBean = new LocalContainerEntityManagerFactoryBean();
       localContainerEntityManagerFactoryBean.setDataSource(dataSource);
       localContainerEntityManagerFactoryBean.setJpaVendorAdapter(new HibernateJpaVendorAdapter());
       localContainerEntityManagerFactoryBean.setPackagesToScan("com.wnx.spring.model");
       Properties properties = new Properties();
       properties.setProperty("hibernate.show_sql","true");
       properties.setProperty("hibernate.format_sql","true");
       properties.setProperty("hibernate.hbm2ddl.auto","update");
       properties.setProperty("hibernate.dialect","org.hibernate.dialect.MySQL57Dialect");
       localContainerEntityManagerFactoryBean.setJpaProperties(properties);
       return localContainerEntityManagerFactoryBean;
   }

   @Bean
    public JpaTransactionManager transactionManager(LocalContainerEntityManagerFactoryBean entityManagerFactoryBean){
       JpaTransactionManager jpaTransactionManager = new JpaTransactionManager();
        jpaTransactionManager.setEntityManagerFactory(entityManagerFactoryBean.getObject());
        return jpaTransactionManager;
    }
    
}
```

