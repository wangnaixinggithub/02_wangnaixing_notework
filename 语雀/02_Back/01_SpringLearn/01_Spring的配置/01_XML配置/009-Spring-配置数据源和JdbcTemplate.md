## 1、SpringJavaConfig

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <context:component-scan base-package="com.wnx.springmvc" use-default-filters="true">
       <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <context:property-placeholder location="classpath*:jdbc.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="driverClassName" value="${jdbc.driverClassName}"/>
        <property name="url" value="${jdbc.url}"/>
    </bean>
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <constructor-arg name="dataSource" ref="dataSource"/>
    </bean>
    <!--配置事务管理器，管理数据源-->
    <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--事务的注解启动-->
    <tx:annotation-driven transaction-manager="dataSourceTransactionManager"/>

</beans>
```

## 2、CRUD

```java
@SpringJUnitConfig(Spring.class)
public class JdbcTemplateTest {
   	@Autowired
    private   JdbcTemplate jdbcTemplate;

    @Test
    public void testDDL(){
        String sql1 = "insert into user values(null,'张三','广西梧州')";
        String sql2 = "delete from user where id =  5";
        String sql3 =  "update user set username = '王五' where id = 3";
        jdbcTemplate.update(sql1);
        jdbcTemplate.update(sql2);
        jdbcTemplate.update(sql3);
    }

    @Test
    public void  testQuery(){
        String sql = "select * from user";
        List<User> userList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(User.class));
        userList.forEach(System.out::println);
    }

}
```

## 3、手动处理结果集映射

```java
@SpringJUnitConfig(Spring.class)
public class JdbcTemplateTest {
   	@Autowired
    private   JdbcTemplate jdbcTemplate;
 	
 	@Test
    public void testQuery2(){
        String sql = "select * from user";
        List<User> userList = jdbcTemplate.query(sql,(ResultSet resultSet, int i)-> {
            int id = resultSet.getInt("id");
            String username = resultSet.getString("username");
            String address = resultSet.getString("address");
            return User.builder().username(username).address(address).id(id).build();
            });
        userList.forEach(System.out::println);
    }
}
```

## 4、批量操作

```java
  @Override
    public void batchUpdate(List<Object[]> batchArgs) {
        String sql = "update t_user set user_name = ?,user_status = ? WHERE user_id = ?";
        jdbcTemplate.batchUpdate(sql,batchArgs);
    }

    @Override
    public void batchDelete(List<Object[]> batchArgs) {
        String sql = "delete from t_user where user_id = ?";
        jdbcTemplate.batchUpdate(sql,batchArgs);
    }

    @Override
    public void batchInsert(List<Object[]> batchArgs) {
        String sql = "insert into t_user(user_id,user_name,user_status) values(?,?,?)";
        jdbcTemplate.batchUpdate(sql,batchArgs);
    }
```

