# 标识什么环境加载什么Bean的注解-@Profile

## 1、配置Profile

>  期望：开发环境使用德鲁伊数据源，生产环境使用DBCP数据源

```java
package com.wnx.spring.config;


import com.alibaba.druid.pool.DruidDataSource;
import org.apache.commons.dbcp2.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;

import javax.sql.DataSource;

@Configuration
@ComponentScan(basePackages = "com.wnx.spring")
public class SpringConfig {

    @Bean("ds")
    @Profile("dev")
    public DataSource devDataSource(){
        DruidDataSource source = new DruidDataSource();
        source.setUsername("root");
        source.setPassword("root");
        source.setUrl("jdbc:mysql://localhost:3306/spring");
        source.setDriverClassName("com.mysql.jdbc.cj.Driver");
        return source;

    }

    @Bean("ds")
    @Profile("prod")
    public DataSource prodDataSource(){
       BasicDataSource source = new BasicDataSource();
        source.setUsername("root");
        source.setPassword("root");
        source.setUrl("jdbc:mysql://localhost:3306/spring");
        source.setDriverClassName("com.mysql.jdbc.cj.Driver");
        return source;

    }

}
```

## 2、激活环境

```java
package com.wnx.spring;

import com.alibaba.druid.pool.DruidDataSource;
import com.wnx.spring.config.SpringConfig;
import org.apache.commons.dbcp2.BasicDataSource;
import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.core.env.ConfigurableEnvironment;

import javax.sql.DataSource;

public class SpringTest {

    @Test
    public void user_devDataSource() {
        //1.创建应用上下文
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        ConfigurableEnvironment environment = context.getEnvironment();

        //2.使用dev环境
        environment.setActiveProfiles("dev");

        //3.加载Spring配置类
        context.register(SpringConfig.class);

        //4.进行应用上下文刷新
        context.refresh();

        //5.获取开发数据源，并打印数据源类型。
        DataSource devDataSource = (DataSource) context.getBean("ds");
        printDataSourceType(devDataSource);  //我是德鲁伊数据源


    }

    @Test
    public void user_prodDataSource() {
        //1.创建应用上下文
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        ConfigurableEnvironment environment = context.getEnvironment();

        //2.使用prod环境
        environment.setActiveProfiles("prod");

        //3.加载Spring配置类
        context.register(SpringConfig.class);

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



