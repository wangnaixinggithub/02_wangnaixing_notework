# Mybatis-批量删除

## 1、UserMapper

```java
public interface UserMapper {
    /**
     * 批量删除
     * @param ids 以逗号分隔的ID组成的字符串
     */
    void deleteByBatchWay01(String ids);


    /**
     * 批量删除
     * @param ids 由ID构成的集合
     */
    void deleteByBatchWay02(List<Integer> ids);

}
```

## 2、UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.UserMapper">

    <!--
        这种时候就不能使用占位符了，直接字符串拼接进去。
    -->
    <delete id="deleteByBatchWay01">
        delete from t_user where id in (${ids})
    </delete>

    <!--
        使用占位符的话，可以通过Mybatis提供的遍历标签，来组装SQL
    -->
    <delete id="deleteByBatchWay02">
        delete from t_user where id in
    <foreach collection="list" open="(" close=")" separator="," item="id">
                #{id}
    </foreach>
    </delete>

</mapper>
```

```java
public class DeleteBatchTest {
    private  UserMapper userMapper;
    @SneakyThrows
    @Before
    public void before(){
         SqlSession sqlSession = SqlSessionFactoryUtil.openSession();
         userMapper = sqlSession.getMapper(UserMapper.class);
    }

    @Test
    public void testDeleteBatchWay01(){
        String ids = "11,12,13";
        userMapper.deleteByBatchWay01(ids);
    }
    @Test
    public void testDeleteBatchWay02(){
        List<Integer> ids = new ArrayList<>();
        ids.add(10);
        ids.add(11);
        ids.add(12);

        userMapper.deleteByBatchWay02(ids);
    }

}
```

