# Spring-XmlConfig-NamedParameterJdbcTemplate

# 1、NamedParameterJdbcTemplate

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
        <context:component-scan base-package="com.wnx.spring" use-default-filters="true">
            <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        </context:component-scan>

        <context:property-placeholder location="classpath*:jdbc.properties"/>
        <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
            <property name="driverClassName" value="${jdbc.driverClassName}"/>
            <property name="url" value="${jdbc.url}"/>
        </bean>
        <bean id="jdbcTemplate" class="org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate">
            <constructor-arg name="dataSource" ref="dataSource"/>
        </bean>

</beans>
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



