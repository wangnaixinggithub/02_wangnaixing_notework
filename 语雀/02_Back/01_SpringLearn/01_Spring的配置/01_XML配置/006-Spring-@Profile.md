# 标识什么环境加载什么Bean的注解-@Profile

## 1、配置Profile

>  期望：开发环境使用德鲁伊数据源，生产环境使用DBCP数据源

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">

  <beans profile="dev">
        <bean id="ds" class="com.alibaba.druid.pool.DruidDataSource">
            <property name="username"  value="root"/>
            <property name="password"  value="root"/>
            <property name="url" value="jdbc:mysql://localhost:3306/spring"/>
            <property name="driverClassName" value="com.mysql.jdbc.cj.Driver"/>
        </bean>
  </beans>


    <beans profile="prod">
        <bean id="ds" class="org.apache.commons.dbcp2.BasicDataSource">
            <property name="username"  value="root"/>
            <property name="password" value="root"/>
            <property name="url" value="jdbc:mysql://localhost:3306/spring"/>
            <property name="driverClassName"  value="com.mysql.jdbc.cj.Driver"/>
        </bean>
    </beans>
                                                         
</beans>


```

## 2、激活环境

```java
package com.wnx.spring;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.commons.dbcp2.BasicDataSource;
import org.junit.jupiter.api.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.env.ConfigurableEnvironment;

import javax.sql.DataSource;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/4/23 14:41
 */


public class SpringTest {

    @Test
    public void user_devDataSource() {
        //1.创建应用上下文
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext();
        ConfigurableEnvironment environment = context.getEnvironment();

        //2.使用dev环境
        environment.setActiveProfiles("dev");

        //3.加载Spring配置类
        context.setConfigLocation("classpath:/bean.xml");

        //4.进行应用上下文刷新
        context.refresh();

        //5.获取开发数据源，并打印数据源类型。
        DataSource devDataSource = (DataSource) context.getBean("ds");
        printDataSourceType(devDataSource);  //我是德鲁伊数据源


    }

    @Test
    public void user_prodDataSource() {
        //1.创建应用上下文
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext();
        ConfigurableEnvironment environment = context.getEnvironment();

        //2.使用prod环境
        environment.setActiveProfiles("prod");

        //3.加载Spring配置类
        context.setConfigLocation("classpath:/bean.xml");

        //4.进行应用上下文刷新
        context.refresh();

        //5.获取开发数据源，并打印数据源类型。
        DataSource devDataSource = (DataSource) context.getBean("ds");
        printDataSourceType(devDataSource);  //我是DBCP数据源
    }


    @Test
    public void printDataSourceType(DataSource dataSource) {
        if (dataSource instanceof DruidDataSource) {
            System.out.println("我是德鲁伊数据源");
        }
        if (dataSource instanceof BasicDataSource) {
            System.out.println("我是DBCP数据源");
        }
    }


}

```



