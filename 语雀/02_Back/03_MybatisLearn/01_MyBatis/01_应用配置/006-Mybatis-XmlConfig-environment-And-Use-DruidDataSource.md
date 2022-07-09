# MyBatis-XmlConfig-environment

# 1、environment

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="jdbc.properties"/>
    <typeAliases>
        <package name="com.wnx.mybatis.pojo"/>
    </typeAliases>
    <environments default="prod">
        <!--开发用的数据池-->
        <environment id="development">
            <transactionManager type="JDBC"/>
             <!--开发用的数据池-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url"
                          value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
         <!--测试用的数据池-->
        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url"
                          value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
        <!--生产用的数据池-->
        <environment id="prod">
            <transactionManager type="JDBC"/>
              <!--整合自定义的德鲁伊数据池工厂-->
            <dataSource type="com.wnx.mybatis.utils.CustomerDruidDataSourceFactory">
                <property name="driverClassName" value="${jdbc.driverClassName}"/>
                <property name="url"
                          value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <package name="com.wnx.mybatis.mapper"/>
    </mappers>
</configuration>
```

# 2、implements DataSourceFactory 

```java
public class CustomerDruidDataSourceFactory implements DataSourceFactory {

    private Properties properties;
    private DataSource dataSource;

    @Override
    public void setProperties(Properties properties) {
        this.properties = properties;
    }
    @Override
    public DataSource getDataSource() {
        try {
             if (dataSource == null){
              dataSource =  DruidDataSourceFactory.createDataSource(properties);
             }
             return dataSource;

        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

# 3、or extends UnpooledDataSourceFactory

```java
package com.wnx.mall.tiny.utils;

import com.alibaba.druid.pool.DruidDataSource;
import org.apache.ibatis.datasource.unpooled.UnpooledDataSourceFactory;

public class DruidDataSourceFactory extends UnpooledDataSourceFactory {
    public DruidDataSourceFactory() {
        this.dataSource=new DruidDataSource();
    }
}
```



