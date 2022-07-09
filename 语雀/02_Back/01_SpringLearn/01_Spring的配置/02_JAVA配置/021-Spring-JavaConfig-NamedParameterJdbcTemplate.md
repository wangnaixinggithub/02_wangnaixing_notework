# Spring-JavaConfig-NamedParameterJdbcTemplate

# 1、NamedParameterJdbcTemplate

```java
package com.wnx.springmvc.config;

import com.alibaba.druid.pool.DruidDataSource;
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
    public NamedParameterJdbcTemplate jdbcTemplate(DataSource dataSource){
        return new NamedParameterJdbcTemplate(dataSource);
    }

   
}
```

```java
@Repository
public class DepartmentDao {
    @Autowired
    private NamedParameterJdbcTemplate jdbcTemplate;


    public void insert(Department department) {
        String sql = "insert into department(id,dept_name)  values(null,:deptName)";
        SqlParameterSource sqlParameterSource = new BeanPropertySqlParameterSource(department);
        jdbcTemplate.update(sql,sqlParameterSource);

    }

    public List<Department> finAll() {
        String sql = "select id,dept_name deptName from department";
        SqlParameterSource sqlParameterSource = new BeanPropertySqlParameterSource(new Department());
        return jdbcTemplate.query(sql, sqlParameterSource, new BeanPropertyRowMapper<>(Department.class));
    }

    public Department findById(Integer id) {
        String sql = "select id,dept_name deptName from department where id = :id";
        SqlParameterSource sqlParameterSource = new BeanPropertySqlParameterSource(new Department(1,""));
        return jdbcTemplate.queryForObject(sql, sqlParameterSource, new BeanPropertyRowMapper<>(Department.class));

    }

    public Long count() {
        String sql = "select count(1) from department";
        SqlParameterSource sqlParameterSource = new BeanPropertySqlParameterSource(new Department());
        return jdbcTemplate.queryForObject(sql, sqlParameterSource,Long.class);
    }
}

```