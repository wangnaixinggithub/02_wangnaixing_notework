# MyBatis-XmlConfig-useGeneratedKeys

- 配置全局策略开启自动获取自增ID

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>


    <!--1.加载resources的jdbc.properties-->
    <properties resource="jdbc.properties"/>

    <!--2.全局设置-->
    <settings>
        <!-- 1.获取自增ID-->
        <setting name="useGeneratedKeys" value="true" />
        <!-- 2.开启驼峰命名转换-->
        <setting name="mapUnderscoreToCamelCase" value="true" />
        <!--3.开启二级缓存-->
        <setting name="cacheEnabled" value="true"/>
    </settings>

    <!--3.数据库环境配置-->
    <environments default="production">
        <!--开发环境-->
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
        <!--生产环境-->
        <environment id="production">
            <transactionManager type="JDBC"/>
            <dataSource type="com.wnx.mall.tiny.utils.DruidDataSourceFactory">
                <property name="driverClassName" value="${jdbc.driverClassName}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--4.注册mapper-->
     <mappers>
         <package name="com.wnx.mall.tiny.mapper"/>
     </mappers>   

</configuration>
```

- 或者在mapper,使用insert 标签属性

```xml
 <!--新增用户-使用useGenerateKey得到用户主键-->
    <insert id="insert2" keyColumn="id" keyProperty="id" useGeneratedKeys="true">
        insert into sys_user(
            user_name,
            user_password,
            user_email,
            user_info,
            head_img,
            create_time
        )
        values(
                  #{userName},
                  #{userPassword},
                  #{userEmail},
                  #{userInfo},
                  #{headImg,jdbcType=BLOB},
                  #{createTime,jdbcType=TIMESTAMP}
              )
    </insert>
```

- 或者在mapper，使用selectkey子标签

```xml
 <!--新增用户-使用selectKey的方式-->
    <insert id="insert3">
        insert into sys_user(
            user_name,
            user_password,
            user_email,
            user_info,
            head_img,
            create_time
        )
        values(
                  #{userName},
                  #{userPassword},
                  #{userEmail},
                  #{userInfo},
                  #{headImg,jdbcType=BLOB},
                  #{createTime,jdbcType=TIMESTAMP}
              )
        <selectKey keyColumn="id" resultType="long" keyProperty="id" order="AFTER">
            SELECT LAST_INSERT_ID()
        </selectKey>
    </insert>
```

```xml
 <!--针对Oracle数据库获取主键的方式-->
    <insert id="insert3">
          <selectKey keyColumn="id" resultType="long" keyProperty="id" order="BEFORE">
            SELECT SQE_ID.mextval from dual
        </selectKey>
        insert into sys_user(
            user_name,
            user_password,
            user_email,
            user_info,
            head_img,
            create_time
        )
        values(
                  #{userName},
                  #{userPassword},
                  #{userEmail},
                  #{userInfo},
                  #{headImg,jdbcType=BLOB},
                  #{createTime,jdbcType=TIMESTAMP}
              )
    </insert>
```

