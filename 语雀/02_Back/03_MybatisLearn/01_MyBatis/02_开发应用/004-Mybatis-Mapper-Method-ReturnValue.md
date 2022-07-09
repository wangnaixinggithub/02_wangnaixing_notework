# MyBatis-ReturnValue

## 1、Mapper

```java
public interface UserMapper {

    /**
     * 返回值为实体类对象
     * @return
     */
    User findAll();
    /**
     * 返回值为实体类对象集合
     * @param username
     * @return
     */
    List<User> findUserByUsername(String username);
    /**
     * 返回值为基本数据类型
     * @return
     */
    Integer findUserCount();
    /**
     * 返回值为Map
     * @param id
     * @return
     */
    Map<String, Object> findById(Integer id);
    /**
     * 返回值为Map的嵌套 
     		key = 结果字段名为id
     		value= 记录列名为key,列值为value的Map.
     * @return
     */
    @MapKey("id")
    Map<String, Object> findAllInsertMap();
}
```

## 2、Mapper.xml

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.UserMapper">

    <select id="findUserByUsername" resultType="User">
            select * from t_user where username = #{username}
    </select>
    <!--
            我使用了User实体接收，然后查询的结果不止一条，会报错
    -->
    <select id="findAll" resultType="User">
        select * from t_user
    </select>
    <!--
        Mybatis由许多类型处理器，他们都用了别名 比如integer 相当于结果集类型为java.util.Integer
    -->
    <select id="findUserCount" resultType="integer">
        select count(1) from t_user
    </select>
    <!--
       Mybatis底层得到结果集对象，如果是实体比如User,会根据属性名进行反射赋值。保证属性名与字段名一一对应就可以。
       当然也可以把结果集对象全部封装到一个map中
    -->
    <select id="findById" resultType="map">
        select * from t_user where id  =  #{id}
    </select>
    <!--
        Map中可以继续存Map，我们可以通过@Mapkey("id") id表key字段。
        把得到的多个结果列存在同一Map
    -->
    <select id="findAllInsertMap" resultType="map">
        select * from t_user
    </select>

</mapper>
```

```java
package com.wnx.mybatis.mapper;
public class QueryTest {
    private  UserMapper userMapper;
    @SneakyThrows
    @Before
    public void before(){
         SqlSession sqlSession = SqlSessionFactoryUtil.openSession();
         userMapper = sqlSession.getMapper(UserMapper.class);
    }

    /**
     * 当查询结果是一条的时候可以用List接收也可以用User实体
     */
    @Test
    public void findUserByUsername(){
        String username = "张三1";
       List<User> userList =  userMapper.findUserByUsername(username);
        System.out.println(userList);
    }

    /**
     * org.apache.ibatis.exceptions.TooManyResultsException:
     * Expected one result (or null) to be returned by selectOne(), but found: 11
     *
     * 返回值为User,Mybatis底层走的是selectOne() 查询到多条，自然抛错！
     */
    @Test
    public void testfindAll(){
       User user = userMapper.findAll();
    }
    @Test
    public void findUserCount(){
       Integer count  = userMapper.findUserCount();
        System.out.println("总记录数"+count);
    }
    @Test
    public void testFindFindById(){
       Map<String, Object> resultMap = userMapper.findById(5);
        System.out.println(resultMap);

    }
    @Test
    public void testFindAllInsertMap(){
        Map<String, Object> resultMap = userMapper.findAllInsertMap();
        System.out.println(resultMap);
    }

}
```

