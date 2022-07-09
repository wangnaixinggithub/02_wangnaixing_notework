# MyBatisPlus-XmlConfig-Optimistic-locking

# 1、乐观锁解决多次修改数据不能保证一致性的问题

乐观锁：通过设定一个版本号，更新后该版本号加1，如果再次更新，版本号不一致的话不给修改。解决并发修改问题。

> 乐观锁实现流程

数据库中添加version字段

取出记录时，获取当前version

```sql
SELECT id,`name`,price,`version` FROM product WHERE id=1
```

更新时，version + 1，如果where语句中的version版本不对，则更新失败

```sql
UPDATE product SET price=price+50, `version`=`version` + 1 WHERE id=1 AND
`version`=1
```

# 2、配置

## 2.1、XML

```java
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
        <property name="plugins">
            <array>
                <ref bean="mybatisPlusInterceptor"/>
            </array>
        </property>
    </bean>
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.wnx.mybatisplus.mapper"/>
    </bean>
    <bean id="globalConfig" class="com.baomidou.mybatisplus.core.config.GlobalConfig">
    <property name="dbConfig">
        <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig">
            <property name="tablePrefix" value="t_"/>
            <property name="idType" value="AUTO"/>
        </bean>
    </property>
    </bean>
    <bean id="mybatisPlusInterceptor" class="com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor">
        <property name="interceptors">
            <list>
                <ref bean="paginationInnerInterceptor"/>
                <!--2.在MybatisPlus拦截器中使用它-->
                <ref bean="optimisticLockerInnerInterceptor"/>
            </list>
        </property>
    </bean>
    <bean id="paginationInnerInterceptor" class="com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor">
            <property name="dbType" value="MYSQL"/>
    </bean>
    <!--1.配置乐观锁内置拦截器-->
    <bean id="optimisticLockerInnerInterceptor" class="com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor"/>
</beans>
```

## 2.2、JavaConfig

```java
package com.wnx.mybatisplus.config;

import com.alibaba.druid.pool.DruidDataSource;
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.core.config.GlobalConfig;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.InnerInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
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
import java.util.ArrayList;
import java.util.List;

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
    public MybatisSqlSessionFactoryBean sqlSessionFactory(DataSource dataSource,GlobalConfig globalConfig,MybatisPlusInterceptor mybatisPlusInterceptor) throws IOException {
        MybatisSqlSessionFactoryBean sqlSessionFactoryBean = new MybatisSqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        sqlSessionFactoryBean.setTypeAliasesPackage("com.wnx.mybatisplus.model");
        //3.使用插件
        sqlSessionFactoryBean.setGlobalConfig(globalConfig);
        sqlSessionFactoryBean.setPlugins(mybatisPlusInterceptor);
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
        dbConfig.setIdType(IdType.AUTO);
        globalConfig.setDbConfig(dbConfig);
        return globalConfig;
    }

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(PaginationInnerInterceptor paginationInnerInterceptor,
                                                         OptimisticLockerInnerInterceptor optimisticLockerInnerInterceptor){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        List<InnerInterceptor> list = new ArrayList();
        list.add(paginationInnerInterceptor);
        //2.在MybatisPlus拦截器中使用它
        list.add(optimisticLockerInnerInterceptor);
        mybatisPlusInterceptor.setInterceptors(list);
        return mybatisPlusInterceptor;
    }

    @Bean
    public PaginationInnerInterceptor paginationInnerInterceptor(){
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor();
        paginationInnerInterceptor.setDbType(DbType.MYSQL);
        return paginationInnerInterceptor;
    }
    @Bean
    public OptimisticLockerInnerInterceptor optimisticLockerInnerInterceptor(){
        //1.配置乐观锁内置拦截器
        return new OptimisticLockerInnerInterceptor();
    }
}

```

# 3、@Version

```java
package com.wnx.mybatisplus.model;

import com.baomidou.mybatisplus.annotation.Version;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/13 9:45
 */
@AllArgsConstructor
@NoArgsConstructor
@Data
public class Product {
    private Long id;
    private String name;
    private Integer price;
    @Version
    private Integer version;
}

```

