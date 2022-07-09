# SpringBoot-Multiple-DataSources

# 1、Use JdbcTemplate

```yaml
spring:
  datasource:
    one:
      username: root
      url: jdbc:mysql://127.0.0.1:3306/spring?useUnicode=true&useJDBCCompliantTimezoneShift=true&serverTimezone=UTC&characterEncoding=utf8
      password: root
      driver-class-name: com.mysql.cj.jdbc.Driver
      type: com.alibaba.druid.pool.DruidDataSource
    two:
      username: root
      url: jdbc:mysql://127.0.0.1:3306/spring2?useUnicode=true&useJDBCCompliantTimezoneShift=true&serverTimezone=UTC&characterEncoding=utf8
      password: root
      driver-class-name: com.mysql.cj.jdbc.Driver
      type: com.alibaba.druid.pool.DruidDataSource
```

```java
@Configuration
public class DataSourceConfig {
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource.one")
    public DataSource dataSourceOne(){
        return DruidDataSourceBuilder.create().build();
    }
    
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource.two")
    public DataSource dataSourceTwo(){
        return DruidDataSourceBuilder.create().build();
    }
}
```

```java
@Configuration
public class JdbcTemplateConfig {
    @Bean
    public JdbcTemplate jdbcTemplateOne(@Qualifier("dataSourceOne") DataSource dataSource){
      return   new JdbcTemplate(dataSource);
    }

    @Bean
    public JdbcTemplate jdbcTemplateTwo(@Qualifier("dataSourceTwo") DataSource dataSource){
        return   new JdbcTemplate(dataSource);
    }
}
```

```java
@Repository
public class UserDaoImpl implements UserDao {
    @Autowired
    @Qualifier("jdbcTemplateOne")
    private JdbcTemplate jdbcTemplate;

    @Resource(name = "jdbcTemplateTwo")
    private JdbcTemplate jdbcTemplateTwo;
 }
```

# 2、Use MyBatis

```yaml
spring:
  datasource:
      one:
        username: root
        url: jdbc:mysql://127.0.0.1:3306/spring?useUnicode=true&useJDBCCompliantTimezoneShift=true&serverTimezone=UTC&characterEncoding=utf8
        password: root
        driver-class-name: com.mysql.cj.jdbc.Driver
        type: com.alibaba.druid.pool.DruidDataSource
      two:
        username: root
        url: jdbc:mysql://127.0.0.1:3306/spring2?useUnicode=true&useJDBCCompliantTimezoneShift=true&serverTimezone=UTC&characterEncoding=utf8
        password: root
        driver-class-name: com.mysql.cj.jdbc.Driver
        type: com.alibaba.druid.pool.DruidDataSource
mybatis:
    configuration:
      map-underscore-to-camel-case: true
      log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    type-aliases-package: com.wnx.boot.model
```

```java
@Configuration
public class DatasourceConfig {
    @Bean
    @ConfigurationProperties("spring.datasource.one")
    public DataSource dataSource1(){
        return DruidDataSourceBuilder.create().build();
    }
    
    @Bean
    @ConfigurationProperties("spring.datasource.two")
    public DataSource dataSource2(){
        return DruidDataSourceBuilder.create().build();
    }
}
```

```java
// MybatisOneConfig.java
@Configuration
@MapperScan(basePackages = "com.wnx.boot.mapper1",sqlSessionFactoryRef = "sqlSessionFactory1",sqlSessionTemplateRef = "sqlSessionTemplate1")
public class MybatisOneConfig {
    @Resource(name = "dataSource1")
    DataSource dataSource;
    @SneakyThrows
    @Bean
    public SqlSessionFactory sqlSessionFactory1(){
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        return sqlSessionFactoryBean.getObject();
    }
    @Bean
    public SqlSessionTemplate sqlSessionTemplate1(){
        return   new SqlSessionTemplate(sqlSessionFactory1());
    }
}

// MybatisTwoConfig.java
@Configuration
@MapperScan(value = "com.wnx.boot.mapper2",sqlSessionTemplateRef = "sqlSessionTemplate2",sqlSessionFactoryRef = "sqlSessionFactory2")
public class MybatisTwoConfig {
    @Resource(name = "dataSource2")
    private DataSource dataSource;
    @SneakyThrows
    @Bean
    public SqlSessionFactory sqlSessionFactory2(){
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        return sqlSessionFactoryBean.getObject();
    }
    @Bean
    public SqlSessionTemplate sqlSessionTemplate2(){
        return new SqlSessionTemplate(sqlSessionFactory2());
    }
}
```

![image-20220515173620294](014-SpringBoot-Multiple-DataSource.assets/image-20220515173620294.png)

```xml
<mapper namespace="com.wnx.boot.mapper1.UserMapperOne">
<mapper namespace="com.wnx.boot.mapper2.UserMapperTwo">
```

