# Mybatis-获取新增记录的主键

## 1、UserMapper

```java
package com.wnx.mybatis.mapper;

import com.wnx.mybatis.pojo.User;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/2/23 23:19
 */
public interface UserMapper {
    /**
     * 使用insert标签的属性
     * @param user
     */
    void insertUserWay01(User user);

    /**
     * 使用selectKey
     * @param user
     */
    void insertUserWay02(User user);


}

```

## 2、UserMapper.xml

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.UserMapper">
    <!--
       1.开启获取新增记录的主键
       2.把主键值赋值给Usr实体的id属性。
    -->
   <insert id="insertUserWay01" useGeneratedKeys="true" keyProperty="id">
       insert into t_user values (null,#{username},#{password},#{age},#{sex},#{email})
   </insert>

    <!--
            和上述一致，另一种写法！！
    -->
    <insert id="insertUserWay02">
        insert into t_user values (null,#{username},#{password},#{age},#{sex},#{email})
        <selectKey keyProperty="id" order="AFTER" resultType="Integer">
            SELECT LAST_INSERT_ID()
        </selectKey>
    </insert>

</mapper>
```

```java
package com.wnx.mybatis.mapper;

import com.wnx.mybatis.pojo.User;
import com.wnx.mybatis.utils.SqlSessionFactoryUtil;
import lombok.SneakyThrows;
import org.apache.ibatis.session.SqlSession;
import org.junit.Before;
import org.junit.Test;

import java.sql.*;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/2/24 22:43
 */
public class GetGeneratedKeysTest {
    private  UserMapper userMapper;
    @SneakyThrows
    @Before
    public void before(){
         SqlSession sqlSession = SqlSessionFactoryUtil.openSession();
         userMapper = sqlSession.getMapper(UserMapper.class);
    }


    //JDBC获取新增记录的主键， 也是Mybatis的底层代码！！！
    @SneakyThrows
    @Test
    public void JdbcUseGeneratedKey(){
        Class.forName("com.mysql.cj.jdbc.Driver");
        String user = "root";
        String password = "root";
        String url = "jdbc:mysql://localhost:3306/mybatis";
        Connection connection = DriverManager.getConnection(url,user,password);
        String sql = "insert into t_user values (null,?,?,?,?,?)";
        PreparedStatement prepareStatement = connection.prepareStatement(sql,Statement.RETURN_GENERATED_KEYS);
        User userModel = User.builder().username("lisi").password("123456").age(18).sex("男").email("1234@qq.com").build();

        prepareStatement.setString(1,userModel.getUsername());
        prepareStatement.setString(2,userModel.getPassword());
        prepareStatement.setInt(3,userModel.getAge());
        prepareStatement.setString(4,userModel.getSex());
        prepareStatement.setString(5,userModel.getEmail());
        int reslut = prepareStatement.executeUpdate();
        ResultSet resultSet = prepareStatement.getGeneratedKeys();

        while (resultSet.next()) {
            System.out.println("新增记录的主键-》"+resultSet.getInt(1));
            userModel.setId(resultSet.getInt(1));
        }

        System.out.println(userModel);


    }

    @Test
    public void insertUserWay01(){
        User userModel = User.builder().username("lisi").password("123456").age(18).sex("男").email("1234@qq.com").build();
        userMapper.insertUserWay01(userModel);
        System.out.println("新增记录的主键-》"+userModel.getId());
        System.out.println(userModel);
    }

    @Test
    public void insertUserWay02(){
        User userModel = User.builder().username("lisi").password("123456").age(18).sex("男").email("1234@qq.com").build();
        userMapper.insertUserWay02(userModel);
        System.out.println("新增记录的主键-》"+userModel.getId());
        System.out.println(userModel);
    }



}

```

