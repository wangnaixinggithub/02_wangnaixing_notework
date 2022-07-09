# MyBatis-Get-Mapper-Method-Arg

## 1、单个字面量类型的参数

若mapper接口中的方法参数为单个的字面量类型 此时可以使用${}和#{}以任意的名称获取参数的值，注意${}需要手动加单引号.

## 2、多个字面量类型的参数

若mapper接口中的方法参数为多个时 此时MyBatis会自动将这些参数放在一个map集合中，以arg0,arg1...为键，以参数为值；以 param1,param2...为键，以参数为值；因此只需要通过${}和#{}访问map集合的键就可以获取相对应的 值，注意${}需要手动加单引号

## 3、map集合类型的参数

若mapper接口中的方法需要的参数为多个时，此时可以手动创建map集合，将这些数据放在map中 只需要通过${}和#{}访问map集合的键就可以获取相对应的值，注意${}需要手动加单引号

## 4、实体类类型的参数

若mapper接口中的方法参数为实体类对象时 此时可以使用${}和#{}，通过访问实体类对象中的属性名获取属性值，注意${}需要手动加单引号

## 5、@Param

可以通过@Param注解标识mapper接口中的方法参数 此时，会将这些参数放在map集合中，以@Param注解的value属性值为键，以参数为值；以 param1,param2...为键，以参数为值；只需要通过${}和#{}访问map集合的键就可以获取相对应的值， 注意${}需要手动加单引号

```java
package com.wnx.mybatis.mapper;

import com.wnx.mybatis.pojo.User;
import org.apache.ibatis.annotations.Param;

import java.util.Map;


public interface UserMapper {

    User findUserByUsername(String username);

    User findUserByUsernameAndPassword(String username,String password);

    User findUserByMap(Map<String, Object> map);

    User findUserByPojo(User user);

    User findUserByNameSpaceAnno(@Param("username")String username,@Param("password")String password);


}

```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.UserMapper">

    <!--一个参数时,只关心值不关心名字。#{} 括号里头名字随便！-->
    <select id="findUserByUsername" resultType="User">
        select * from t_user where username = #{xxx}
    </select>

    <!--两个以上参数，Mybatis会把他封装到一个Map中，
    按照方法参数顺序，给他们起arg0,arg1或者 param1 param2作为值的key-->
    <select id="findUserByUsernameAndPassword" resultType="User">
        select * from t_user where username = #{arg0} and password = #{param2}
    </select>

    <!--既然Mybatis底层是把参数封装到Map中，我们也自己提供一个参数Map，好处在于key由我们定！-->
    <select id="findUserByMap" resultType="User">
        select * from t_user where username = #{username} and password = #{password}
    </select>

    <!--如果POJO，会通过属性名的set方法反射到Mybatis的Map中，属性名为key!-->
    <select id="findUserByPojo" resultType="User">
        select * from t_user where username = #{username} and password = #{password}
    </select>

    <!--再到后来，Mybatis提供了命名空间的注解，把注解的Value值作为Mybatis参数的Map的key,把值进行封装-->
    <select id="findUserByNameSpaceAnno" resultType="User">
        select * from t_user where username = #{username} and password = #{password}
    </select>

</mapper>
```





