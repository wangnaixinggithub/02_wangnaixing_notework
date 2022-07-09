# SpringMvc-JavaConfig-`tx:annotation-driven`

# 1、t`x:annotation-driven`

```java
@Configuration
@ComponentScan(
        basePackages = "com.wnx.springmvc",
        excludeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,classes = Controller.class)})
@PropertySource("classpath:/jdbc.properties")
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
    public JdbcTemplate jdbcTemplate(DataSource dataSource){
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

}
```

# 2、 @Transactional

```java
package com.wnx.spring.service;
@Service
public class BookService {

    @Resource
    private BookDao bookDao;
    
    @Transactional(rollbackFor = Exception.class)
    public void checkOut(String username,String isbn){
        //1.减库存
        bookDao.updateStock(isbn);

        //2.余额
        Integer bookPrice = bookDao.getBookPrice(isbn);
        
        bookDao.updateBalance(username,bookPrice);
    }
}
```