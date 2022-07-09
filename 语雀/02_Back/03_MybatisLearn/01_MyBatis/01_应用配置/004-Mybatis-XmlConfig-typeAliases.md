# Mybatis-XmConfig-typeAliases

## 1、typeAlias

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="jdbc.properties"/>
    <!--指定别名-->
    <typeAliases>
        <!--这个类com.wnx.mybatis.pojo.User，在mapper中可以用叫User-->
        <typeAlias type="com.wnx.mybatis.pojo.User" alias="User"/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url"
                          value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/wnx/mybatis/mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

## 2、 resultType="User"

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.UserMapper">
    <!--使用别名：resultType="User"-->
    <select id="getAll" resultType="User">
        select * from t_user
    </select>
    <select id="getById" resultType="User" >
        select * from t_user where id = 1
    </select>
</mapper>
```

## 3、package

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="jdbc.properties"/>
    <!--指定别名-->
    <typeAliases>
        <!--扫com.wnx.mybatis.pojo包下所有类，给类起名字为类名-->
        <package name="com.wnx.mybatis.pojo"/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url"
                          value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/wnx/mybatis/mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

