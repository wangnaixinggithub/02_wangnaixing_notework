# MyBatisPlus-Customize-Page

# 1、Extend Page

```java
@Data
@Accessors(chain = true)
@EqualsAndHashCode(callSuper = true)
public class MyPage<T> extends Page<T> {
    public MyPage(long current, long size) {
        super(current, size);
    }
}

```

- 设想的查询条件

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class ParamSome {
    private Integer param1;
    private String param2;
}
```

# 3、Use It

-  3.x的page可以进行取值，多个入参记得加上注解！

- 自定义的page类，必须放在入参第一位

- 返回值可以用`IPage<T>`接收，也可以使用入参的`MyPage<T>`接收

  

```java
public interface UserMapper extends BaseMapper<User> {
    MyPage<User> mySelectPage(@Param("myPage") MyPage<User> myPage,@Param("paramSome") ParamSome paramSome);
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mall.tiny.modules.test.mapper.UserMapper">

    <!-- 通用查询映射结果 -->
    <resultMap id="BaseResultMap" type="com.wnx.mall.tiny.modules.test.model.User">
        <id column="id" property="id" />
        <result column="name" property="name" />
        <result column="age" property="age" />
        <result column="email" property="email" />
    </resultMap>
    
    <select id="mySelectPage" resultType="com.wnx.mall.tiny.modules.test.model.User">
        select * from user
            where  age = #{paramSome.param1} and name = #{paramSome.param2}
    </select>
</mapper>

```

# 4、Validation Test

```java
@Test
public void test(){
     	ParamSome paramSome = new ParamSome(20, "Jack");
        MyPage<User> page6 = mapper.mySelectPage(new MyPage<User>(1, 5).setSelectInt(20).setSelectStr("Jack"),
                paramSome);
        log.error("总条数 -------------> {}", page6.getTotal());
        log.error("当前页数 -------------> {}", page6.getCurrent());
        log.error("当前每页显示数 -------------> {}", page6.getSize());
        page6.getRecords().forEach(System.out::println);
}     
```

```javascript
Preparing: SELECT COUNT(1) FROM user WHERE age = ? AND name = ? OR age = ? AND name = ? 
Parameters: 20(Integer), Jack(String), 20(Integer), Jack(String)
Preparing: select * from user where age = ? and name = ? or age = ? and name = ? LIMIT ?,? 
Parameters: 20(Integer), Jack(String), 20(Integer), Jack(String), 0(Long), 5(Long)

2022-01-11 18:30:55.227 ERROR 13848 --- [           main] com.wnx.mall.tiny.PaginationTest         : 总条数 -------------> 13
2022-01-11 18:30:55.228 ERROR 13848 --- [           main] com.wnx.mall.tiny.PaginationTest         : 当前页数 -------------> 1
2022-01-11 18:30:55.228 ERROR 13848 --- [           main] com.wnx.mall.tiny.PaginationTest         : 当前每页显示数 -------------> 5

User(id=2, name=Jack, age=20, email=test2@baomidou.com)
User(id=3, name=Jack, age=20, email=test2@baomidou.com)
User(id=4, name=Jack, age=20, email=test2@baomidou.com)
User(id=5, name=Jack, age=20, email=test2@baomidou.com)
User(id=6, name=Jack, age=20, email=test2@baomidou.com)

```

