# MyBatis-Hello World

```xml
<!--pom.xml-->   
<dependencies>
       <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.7</version>
       </dependency>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.27</version>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <version>1.18.22</version>
       </dependency>
   </dependencies>
```

```xml
 <!-- mybatis-config.xml -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--设置连接数据库的环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url"
                          value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <mapper resource="com/wnx/mybatis/mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

```xml
<!--UserMapper.xml-->
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.UserMapper">
    <!--int insertUser();-->
    <insert id="insertUser">
        insert into t_user values(null,'张三','123',23,'女','123@qq.com')
    </insert>
</mapper>
```

```java
package com.wnx.mybatis.mapper;

public interface UserMapper {

    int insertUser();
}
```

```java
public class UserMapperTest {

    @SneakyThrows
    @Test
    public void helloWorld(){
        //1.读取全局配置文件
        InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
        //2.创建Sql会话工厂构建器，由此创建出Sql会话工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = builder.build(stream);
        
        //3.开启一次会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
		//4.获取UserMapper接口代理
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        
        //5.执行一次数据库操作
        userMapper.insertUser();
        
        //6.提交会话
        sqlSession.commit();
        
        //7.关闭会话
        sqlSession.close();

    }
}

```

