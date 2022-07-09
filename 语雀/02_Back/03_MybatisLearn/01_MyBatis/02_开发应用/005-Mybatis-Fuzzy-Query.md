# Mybatis-模糊查询

## UserMapper

```java
public interface UserMapper {

    /**
     * 根据用户名进行模糊查询 采用${}的方式
     * @param username
     * @return
     */
    List<User> findUserByUserNameLikeWay01(String username);

    /**
     * 根据用户名进行模糊查询 采用Mysql的concat()函数
     * @param username
     * @return
     */
    List<User> findUserByUserNameLikeWay02(String  username);

    /**
     * 根据用户名进行模糊查询 采用 "%"#{username}"%" 的方式
     * @param username
     * @return
     */
    List<User> findUserByUserNameLikeWay03(String username);


}

```

## UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.UserMapper">

    <!--${},输入直接字符串拼接的方式，我们能给他加上''和 %-->
   <select id="findUserByUserNameLikeWay01" resultType="User">
       select * from t_user where username like '%${username}%'
   </select>
    
    <!--首先，#{}，属于占位符拼接了,#{},Mybatis会帮我们加上''
        其次，MySQl中存在concat函数，能帮我们把两个%加上
        等价与 '%' + '张三1' + '%'  =   '%张三1%'
    -->
    <select id="findUserByUserNameLikeWay02" resultType="User">
        select * from t_user where username  like  concat('%',#{username},'%')
    </select>
    
    <!--
        推荐使用这种方式！！！
    -->
    <select id="findUserByUserNameLikeWay03" resultType="User">
        select * from t_user where username  like  "%"#{username}"%"
    </select>

</mapper>
```

