# MyBatis-Use-CRUD

1、添加

```java
    <!--int insertUser();-->
    <insert id="insertUser">
        insert into t_user values(null,'张三','123',23,'女','123@qq.com')
    </insert>
```

2、删除

```java
    <!-- int  deleteUser()-->
    <delete id="deleteUser">
        delete from t_user where id = 5
    </delete>
```

3、修改

```java
    <!--    int updateUser();-->
    <update id="updateUser">
        update t_user set username = '张三' where id = 2
    </update>
```

4、查询一个实体类对象

```java
   <!-- User getById();-->
    <select id="getById" resultType="com.wnx.mybatis.pojo.User" >
        select * from t_user where id = 1
    </select>
```

5、查询集合

```java
   <!--List<User> getAll()-->
    <select id="getAll" resultType="com.wnx.mybatis.pojo.User">
        select * from t_user
    </select>
```

> 当查询的数据为多条时，不能使用实体类作为返回值，只能使用集合，否则会抛出异常 TooManyResultsException；
>
>  但是若查询的数据只有一条，可以使用实体类或集合作为返回值

