# Mybatis-Use-JdbcType-SET-DateType

> 在学习Mysql的时候，我们知道数据库类型有date,datatime,time类型。
>
> 在用Mybatis进行插入数据的时候，我们实体一般都是直接指定java.util.Date类型。
>
> 为了确保数据类型插入的准确性，有时需要Mapper中指定日期类型

> Java中的数据类型和Mysql中如何映射的？Mybatis如下面这边展示，箭头表示对应关系。

- (java)Date => (mysql)date `mysql实例：2021-12-13`
- Time => time `mysql实例：12：12`
- TimeSTAMP => datetime `mysql实例：2021-12-13 12：12`

# 1、JdbcType = 默认

```xml-dtd
    <!--新增用户-->
    <insert id="insert" parameterType="SysUser">
        insert into sys_user(
                             id,
                             user_name,
                             user_password,
                             user_email,
                             user_info,
                             head_img,
                             create_time
                            )
                    values(
                            #{id},
                           #{userName},
                           #{userPassword},
                           #{userEmail},
                           #{userInfo},
                           #{headImg,jdbcType=BLOB},
                           #{createTime}
                         )
    </insert>
```

```javascript
DEBUG [main] - ==>  Preparing: insert into sys_user( id, user_name, user_password, user_email, user_info, head_img, create_time ) values( ?, ?, ?, ?, ?, ?, ? ) 
DEBUG [main] - ==> Parameters: null, test1(String), 123456(String), test@mybatis.tk(String), test info(String), java.io.ByteArrayInputStream@52102734(ByteArrayInputStream), 2021-12-13 10:24:33.064(Timestamp)
DEBUG [main] - <==    Updates: 1
```

# 2、javaType=DATE

```xml-dtd
    <!--新增用户-->
    <insert id="insert" parameterType="SysUser">
        insert into sys_user(
                             id,
                             user_name,
                             user_password,
                             user_email,
                             user_info,
                             head_img,
                             create_time
                            )
                    values(
                            #{id},
                           #{userName},
                           #{userPassword},
                           #{userEmail},
                           #{userInfo},
                           #{headImg,jdbcType=BLOB},
                           #{createTime,jdbcType=DATE}
                         )
    </insert>
```

```java
DEBUG [main] - ==>  Preparing: insert into sys_user( id, user_name, user_password, user_email, user_info, head_img, create_time ) values( ?, ?, ?, ?, ?, ?, ? ) 

DEBUG [main] - ==> Parameters: null, test1(String), 123456(String), test@mybatis.tk(String), test info(String), java.io.ByteArrayInputStream@52102734(ByteArrayInputStream), 2021-12-13(Date)

DEBUG [main] - <==    Updates: 1
```

# 3、JavaType=Time

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211213103058.jpg)

```xml-dtd
    <!--新增用户-->
    <insert id="insert" parameterType="SysUser">
        insert into sys_user(
                             id,
                             user_name,
                             user_password,
                             user_email,
                             user_info,
                             head_img,
                             create_time
                            )
                    values(
                            #{id},
                           #{userName},
                           #{userPassword},
                           #{userEmail},
                           #{userInfo},
                           #{headImg,jdbcType=BLOB},
                           #{createTime,jdbcType=TIME}
                         )
    </insert>
```

```java
DEBUG [main] - ==>  Preparing: insert into sys_user( id, user_name, user_password, user_email, user_info, head_img, create_time ) values( ?, ?, ?, ?, ?, ?, ? ) 

DEBUG [main] - ==> Parameters: null, test1(String), 123456(String), test@mybatis.tk(String), test info(String), java.io.ByteArrayInputStream@52102734(ByteArrayInputStream), 10:31:46(Time)

DEBUG [main] - <==    Updates: 1
```

# 4、jdbcType=TIMESTAMP

```xml-dtd
    <!--新增用户-->
    <insert id="insert" parameterType="SysUser">
        insert into sys_user(
                             id,
                             user_name,
                             user_password,
                             user_email,
                             user_info,
                             head_img,
                             create_time
                            )
                    values(
                            #{id},
                           #{userName},
                           #{userPassword},
                           #{userEmail},
                           #{userInfo},
                           #{headImg,jdbcType=BLOB},
                           #{createTime,jdbcType=TIMESTAMP}
                         )
    </insert>
```

```java
DEBUG [main] - ==>  Preparing: insert into sys_user( id, user_name, user_password, user_email, user_info, head_img, create_time ) values( ?, ?, ?, ?, ?, ?, ? ) 

DEBUG [main] - ==> Parameters: null, test1(String), 123456(String), test@mybatis.tk(String), test info(String), java.io.ByteArrayInputStream@3541cb24(ByteArrayInputStream), 2021-12-13 10:33:53.337(Timestamp)

DEBUG [main] - <==    Updates: 1
```

