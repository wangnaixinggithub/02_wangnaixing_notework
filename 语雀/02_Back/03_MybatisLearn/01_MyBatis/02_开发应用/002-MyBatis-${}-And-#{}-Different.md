# MyBatis-${}-And-#{}-Different?

- MyBatis获取参数值的两种方式：${}和#{}

- ${}的本质就是字符串拼接，#{}的本质就是占位符赋值

- ${}使用字符串拼接的方式拼接SQL，若为字符串类型或日期类型的字段进行赋值时，需要手动加单引 号
- 但是#{}使用占位符赋值的方式拼接SQL，此时为字符串类型或日期类型的字段进行赋值时，MyBatis可以自动添加单引号.

# 1、${},#{} 实现JDBC底层

```java
package com.wnx.mybatis.mapper;

import com.wnx.mybatis.pojo.User;
import com.wnx.mybatis.utils.SqlSessionFactoryUtil;
import lombok.SneakyThrows;
import org.apache.ibatis.session.SqlSession;
import org.junit.Before;
import org.junit.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class ParamTest {
    private  UserMapper userMapper;
    
    @SneakyThrows
    @Before
    public void before(){
         SqlSession sqlSession = SqlSessionFactoryUtil.openSession();
         userMapper = sqlSession.getMapper(UserMapper.class);
    }

    //${} => JDBC底层
    @SneakyThrows
    @Test
    public void testParameterSplicing(){
        String username = "张三1";

        Class.forName("com.mysql.cj.jdbc.Driver");
        String user = "root";
        String password = "root";
        String url = "jdbc:mysql://localhost:3306/mybatis";
        Connection connection = DriverManager.getConnection(url,user,password);

        String sql = "select * from t_user where username =  '"+username+"' ";
        PreparedStatement prepareStatement = connection.prepareStatement(sql);

        ResultSet resultSet = prepareStatement.executeQuery();

    }
    
    //${} => Mybatis 使用
    @Test
    public void testParameterSplicing2(){
        String username = "张三1";
        User user =  userMapper.findUserByUsername1(username);
    }

    //#{} 底层
    @SneakyThrows
    @Test
    public void ParameterPlaceholder(){

        String username = "张三1";

        Class.forName("com.mysql.cj.jdbc.Driver");
        String user = "root";
        String password = "root";
        String url = "jdbc:mysql://localhost:3306/mybatis";
        Connection connection = DriverManager.getConnection(url,user,password);

        String sql = "select * from t_user where username =  ? ";
        PreparedStatement prepareStatement = connection.prepareStatement(sql);
        prepareStatement.setString(1,username);

    }

    //#{} => Mybatis 使用
    @Test
    public void ParameterPlaceholder2(){
        String username = "张三1";
        User user =  userMapper.findUserByUsername2(username);
    }

}
```

# 2、开发使用

```java
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.UserMapper">

    <select id="findUserByUsername1" resultType="User">
        select * from t_user where username = '${username}'
    </select>
    <select id="findUserByUsername2" resultType="User" >
        select * from t_user where username = #{username}
    </select>
</mapper>
```

```cobol
DEBUG 02-27 00:38:09,283 ==>  Preparing: select * from t_user where username = '张三1'  (BaseJdbcLogger.java:137) 
DEBUG 02-27 00:38:09,307 ==> Parameters:   (BaseJdbcLogger.java:137) 
DEBUG 02-27 00:38:09,323 <==      Total: 1  (BaseJdbcLogger.java:137) 
```

```cobol
DEBUG 02-27 00:38:44,012 ==>  Preparing: select * from t_user where username = ?  (BaseJdbcLogger.java:137) 
DEBUG 02-27 00:38:44,039 ==> Parameters: 张三1(String)  (BaseJdbcLogger.java:137) 
DEBUG 02-27 00:38:44,056 <==      Total: 1  (BaseJdbcLogger.java:137) 
```

