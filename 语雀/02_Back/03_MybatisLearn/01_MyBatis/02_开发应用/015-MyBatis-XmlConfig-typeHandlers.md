# MyBatis-XmlConfig-typeHandlers

# 1、mybatis-config.xml

- 通过自定义类型处理器，处理一些自定义类型转化，比如把List<String> => varchar类型，元素以逗号形式分隔。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--1.加载resources的jdbc.properties-->
    <properties resource="jdbc.properties"/>

    <!--2.全局设置-->
    <settings>
        <!-- 1.获取自增ID-->
        <setting name="useGeneratedKeys" value="true" />
        <!-- 2.开启驼峰命名转换-->
        <setting name="mapUnderscoreToCamelCase" value="true" />
        <!--3.启动懒加载的配置-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="true"/>

    </settings>

    <!--3.别名处理器-->
    <typeAliases>
        <package name="com.wnx.mall.tiny.model"/>
    </typeAliases>

    <!--4.类型处理器-->
    <typeHandlers>
        <typeHandler handler="com.wnx.mall.tiny.handler.List2VarcharHandler"/>
    </typeHandlers>

    <!--3.数据库环境配置-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--4.注册mapper-->
     <mappers>
         <package name="com.wnx.mall.tiny.mapper"/>
     </mappers>   

</configuration>
```

# 2、List2VarcharHandler

```java
package com.wnx.mall.tiny.handler;

import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;
import org.apache.ibatis.type.TypeHandler;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Arrays;
import java.util.List;

@MappedJdbcTypes(JdbcType.VARCHAR)
@MappedTypes(List.class)
public class List2VarcharHandler implements TypeHandler<List<String>> {
    
    @Override
    public void setParameter(PreparedStatement preparedStatement, int i, List<String> parameter, JdbcType jdbcType) throws SQLException {
        StringBuilder stringBuffer = new StringBuilder();
        for (String s : parameter) {
            stringBuffer.append(s).append(",");
        }
        preparedStatement.setString(i,stringBuffer.toString());

    }

    @Override
    public List<String> getResult(ResultSet rs, String columnName) throws SQLException {
        String columnValue = rs.getString(columnName);
        if (columnValue != null){
            return Arrays.asList(columnValue.split(","));
        }
        return null;
    }

    @Override
    public List<String> getResult(ResultSet rs, int columnIndex) throws SQLException {
        String columnValue = rs.getString(columnIndex);
        if (columnValue != null){
            return Arrays.asList(columnValue.split(","));
        }
        return null;
    }

    @Override
    public List<String> getResult(CallableStatement cs, int columnIndex) throws SQLException {
        String columnValue = cs.getString(columnIndex);
        if (columnValue != null){
            return Arrays.asList(columnValue.split(","));
        }
        return null;
    }
}
```

# 3、Model

```java
package com.wnx.mall.tiny.model;

import lombok.Data;

import java.io.Serializable;
import java.util.List;
@Data
public class User implements Serializable {
    private Long id;
    private String username;
    private String address;
    private List<String> favorites;
}
```

# 4、UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mall.tiny.mapper.UserMapper">

    <insert id="insert" parameterType="com.wnx.mall.tiny.model.User">
        insert into t_user(id,username,address,favorites) values (null,#{username},#{address},#{favorites,typeHandler=com.wnx.mall.tiny.handler.List2VarcharHandler})
    </insert>

    <select id="findAll" resultType="com.wnx.mall.tiny.model.User">
        select * from t_user
    </select>
</mapper>
```

- 数据库效果：

![image-20220213103502782](D:/%E5%9B%BE%E7%89%87%E7%BB%9F%E4%B8%80%E4%BF%9D%E7%AE%A1%E5%A4%84/image-20220213103502782.png)

- 查询效果：

![image-20220213104001932](D:/%E5%9B%BE%E7%89%87%E7%BB%9F%E4%B8%80%E4%BF%9D%E7%AE%A1%E5%A4%84/image-20220213104001932.png)

```java
public class UserMapperTest {

    @Test
    public void testFindAll(){
        UserMapper userMapper = new UserMapperImpl();
        List<User> userList = userMapper.findAll();
        userList.forEach(System.out::println);
    }

    @Test
    public void insert(){
        //1.构造插入模型
        UserMapper userMapper = new UserMapperImpl();
        List<String> list = new ArrayList<>();
        list.add("打代码");
        list.add("打游戏");
        list.add("睡觉");
        User user = User.builder().username("wnx").address("梧州学院").favorites(list).build();

        //2.执行插入
        userMapper.insert(user);
    }
}

```

