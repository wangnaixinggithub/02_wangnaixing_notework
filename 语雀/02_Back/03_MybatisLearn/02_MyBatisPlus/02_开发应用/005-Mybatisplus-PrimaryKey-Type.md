# Mybatisplus-PrimaryKey-Type

## 1、默认：雪花算法

```java
package com.wnx.mybatisplus.model;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/10 23:29
 */
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Serializable {
    /**
     * 默认就是雪花算法 type = IdType.ASSIGN_ID 可以不配置。
     */
    @TableId(value = "uid",type = IdType.ASSIGN_ID)
    private Long id;
    private String name;
    private Integer age;
    private String email;
}

```

## 2、可配置为:根据数据库ID自增

```java
package com.wnx.mybatisplus.model;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/10 23:29
 */
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Serializable {
    /**
     * 可以设置类型根据ID自增的，设置完了之后一定确保数据库也开启了使用自增策略！
     */
    @TableId(value = "uid",type = IdType.AUTO)
    private Long id;
    private String name;
    private Integer age;
    private String email;
}

```

## 3、全局配置

- xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
    <context:component-scan base-package="com.wnx.mybatisplus" use-default-filters="true">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <context:property-placeholder location="jdbc.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="driverClassName" value="${jdbc.driverClassName}"/>
        <property name="url" value="${jdbc.url}"/>
    </bean>
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <tx:annotation-driven/>
    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="globalConfig" ref="globalConfig"/>
    </bean>
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.wnx.mybatisplus.mapper"/>
    </bean>
    <!--2.全局配置设定表的前缀为t_-->
    <bean id="globalConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig">
    <property name="dbConfig">
        <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig">
            <property name="tablePrefix" value="t_"/>
            <property name="idType" value="AUTO"/>
        </bean>
    </property>
    </bean>

</beans>
```

- javaConfig

```java
package com.wnx.mybatisplus.config;

import com.alibaba.druid.pool.DruidDataSource;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.core.config.GlobalConfig;
import com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean;
import org.mybatis.spring.mapper.MapperScannerConfigurer;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.*;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.stereotype.Controller;
import org.springframework.transaction.TransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;
import java.io.IOException;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/10 23:38
 */
@Configuration
@EnableTransactionManagement
@ComponentScan(basePackages = "com.wnx.mybatisplus",
        useDefaultFilters = true,
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
@PropertySource("classpath:jdbc.properties")
public class SpringConfig {
    @Bean
    public DataSource dataSource( @Value("${jdbc.username}")String username,
                                  @Value("${jdbc.password}")String password,
                                  @Value("${jdbc.driverClassName}")String driverClassName,
                                  @Value("${jdbc.url}")String url){
        System.out.println(username);
        System.out.println(password);
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setUsername(username);
        druidDataSource.setPassword(password);
        druidDataSource.setUrl(url);
        druidDataSource.setDriverClassName(driverClassName);
        return druidDataSource;
    }
    @Bean
    public TransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }

    @Bean
    public MybatisSqlSessionFactoryBean sqlSessionFactory(DataSource dataSource,GlobalConfig globalConfig) throws IOException {
        MybatisSqlSessionFactoryBean sqlSessionFactoryBean = new MybatisSqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        sqlSessionFactoryBean.setTypeAliasesPackage("com.wnx.mybatisplus.model");
        sqlSessionFactoryBean.setGlobalConfig(globalConfig);
        return sqlSessionFactoryBean;
     }
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
         MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
         mapperScannerConfigurer.setBasePackage("com.wnx.mybatisplus.mapper");
         return mapperScannerConfigurer;
     }
    @Bean
    public GlobalConfig globalConfig(){

        GlobalConfig globalConfig = new GlobalConfig();
        GlobalConfig.DbConfig dbConfig = new GlobalConfig.DbConfig();
        dbConfig.setTablePrefix("t_");
        //设置为数据库自增策略
        dbConfig.setIdType(IdType.AUTO);
        globalConfig.setDbConfig(dbConfig);
        return globalConfig;

    }
}

```

```java
package com.wnx.mybatisplus.config;

import com.alibaba.druid.pool.DruidDataSource;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.core.config.GlobalConfig;
import com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean;
import org.mybatis.spring.mapper.MapperScannerConfigurer;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.*;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.stereotype.Controller;
import org.springframework.transaction.TransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;
import java.io.IOException;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/10 23:38
 */
@Configuration
@EnableTransactionManagement
@ComponentScan(basePackages = "com.wnx.mybatisplus",
        useDefaultFilters = true,
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
@PropertySource("classpath:jdbc.properties")
public class SpringConfig {
    @Bean
    public DataSource dataSource( @Value("${jdbc.username}")String username,
                                  @Value("${jdbc.password}")String password,
                                  @Value("${jdbc.driverClassName}")String driverClassName,
                                  @Value("${jdbc.url}")String url){
        System.out.println(username);
        System.out.println(password);
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setUsername(username);
        druidDataSource.setPassword(password);
        druidDataSource.setUrl(url);
        druidDataSource.setDriverClassName(driverClassName);
        return druidDataSource;
    }
    @Bean
    public TransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }

    @Bean
    public MybatisSqlSessionFactoryBean sqlSessionFactory(DataSource dataSource,GlobalConfig globalConfig) throws IOException {
        MybatisSqlSessionFactoryBean sqlSessionFactoryBean = new MybatisSqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        sqlSessionFactoryBean.setTypeAliasesPackage("com.wnx.mybatisplus.model");
        sqlSessionFactoryBean.setGlobalConfig(globalConfig);
        return sqlSessionFactoryBean;
     }
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
         MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
         mapperScannerConfigurer.setBasePackage("com.wnx.mybatisplus.mapper");
         return mapperScannerConfigurer;
     }
    @Bean
    public GlobalConfig globalConfig(){

        GlobalConfig globalConfig = new GlobalConfig();
        GlobalConfig.DbConfig dbConfig = new GlobalConfig.DbConfig();
        dbConfig.setTablePrefix("t_");
        //设置为数据库自增策略
        dbConfig.setIdType(IdType.AUTO);
        globalConfig.setDbConfig(dbConfig);
        return globalConfig;

    }
}

```

