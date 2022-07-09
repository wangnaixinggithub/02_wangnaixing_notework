# MyBatis-Use-Dynamic SQL

# 1、Need Config

```xml
<dependencies>
    <!--SpringBoot整合MyBatis-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.1.3</version>
    </dependency>
    
    <!--MyBatis分页插件-->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper-spring-boot-starter</artifactId>
        <version>1.3.0</version>
    </dependency>
    
    <!--集成druid连接池-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.1.10</version>
    </dependency>
    
    <!-- MyBatis 生成器 -->
    <dependency>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-core</artifactId>
        <version>1.4.0</version>
    </dependency>
    
    <!-- MyBatis 动态SQL支持 -->
    <dependency>
        <groupId>org.mybatis.dynamic-sql</groupId>
        <artifactId>mybatis-dynamic-sql</artifactId>
        <version>1.2.1</version>
    </dependency>
    
    <!--Mysql数据库驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.15</version>
    </dependency>
    
</dependencies>
```

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root

mybatis:
  mapper-locations:
    - classpath:dao/*.xml
```

```java
package com.wnx.mall.tiny.config;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;

/**
 * @author wangnaixing
 * @Classname MyBatisConfig
 * @Description TODO  MyBatis配置类
 * @Date 2021/12/6 23:23
 * @Created by wangnaixing
 */
@Configuration
@MapperScan({"com.wnx.mall.tiny.mbg.mapper","com.wnx.mall.tiny.dao"})
public class MyBatisConfig {
}
```

```properties
#generator.properties
jdbc.driverClass=com.mysql.cj.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://localhost:3306/mall-tiny-dynamic-sql?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
jdbc.userId=root
jdbc.password=root
```

```xml
<!--generatorConfig.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <properties resource="generator.properties"/>
    <context id="MySqlContext" targetRuntime="MyBatis3DynamicSQL">
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
        <property name="javaFileEncoding" value="UTF-8"/>
        <!-- 为模型生成序列化方法-->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
        <!-- 为生成的Java模型创建一个toString方法 -->
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>
        <!--可以自定义生成model的代码注释-->
        <commentGenerator type="com.wnx.mall.tiny.mbg.CommentGenerator">
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
            <property name="suppressDate" value="true"/>
            <property name="addRemarkComments" value="true"/>
        </commentGenerator>
        <!--配置数据库连接-->
        <jdbcConnection driverClass="${jdbc.driverClass}"
                        connectionURL="${jdbc.connectionURL}"
                        userId="${jdbc.userId}"
                        password="${jdbc.password}">
            <!--解决mysql驱动升级到8.0后不生成指定数据库代码的问题-->
            <property name="nullCatalogMeansCurrent" value="true" />
        </jdbcConnection>
        <!--指定生成model的路径-->
        <javaModelGenerator targetPackage="com.wnx.mall.tiny.mbg.model" targetProject="src\main\java"/>
        <!--指定生成mapper接口的的路径-->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.wnx.mall.tiny.mbg.mapper"
                             targetProject="src\main\java"/>
        <!--生成全部表tableName设为%-->
        <table tableName="ums_admin">
            <generatedKey column="id" sqlStatement="MySql" identity="true"/>
        </table>
        <table tableName="ums_role">
            <generatedKey column="id" sqlStatement="MySql" identity="true"/>
        </table>
        <table tableName="ums_admin_role_relation">
            <generatedKey column="id" sqlStatement="MySql" identity="true"/>
        </table>
    </context>
</generatorConfiguration>
```

- 与之前使用MBG有所不同，`targetRuntime`需要改为`MyBatis3DynamicSql`，用于配置生成mapper.xml路径的`sqlMapGenerator`标签也不需要配置了；

```java
public class CommentGenerator extends DefaultCommentGenerator {
    @Override
    public void addFieldComment(Field field, IntrospectedTable introspectedTable,
                                IntrospectedColumn introspectedColumn) {
        String remarks = introspectedColumn.getRemarks();
        //根据参数和备注信息判断是否添加备注信息
        if(addRemarkComments&&StringUtility.stringHasValue(remarks)){
            //数据库中特殊字符需要转义
            if(remarks.contains("\"")){
                remarks = remarks.replace("\"","'");
            }
            //给model的字段添加swagger注解
            field.addJavaDocLine("@ApiModelProperty(value = \""+remarks+"\")");
        }
    }
}
```

```java
public class CommentGenerator extends DefaultCommentGenerator {

    @Override
    public void addFieldAnnotation(Field field, IntrospectedTable introspectedTable, IntrospectedColumn introspectedColumn, Set<FullyQualifiedJavaType> imports) {
        if (!addRemarkComments || CollUtil.isEmpty(imports)) return;
        long count = imports.stream()
                .filter(item -> API_MODEL_PROPERTY_FULL_CLASS_NAME.equals(item.getFullyQualifiedName()))
                .count();
        if (count <= 0L) {
            return;
        }
        String remarks = introspectedColumn.getRemarks();
        //根据参数和备注信息判断是否添加备注信息
        if (StringUtility.stringHasValue(remarks)) {
            //数据库中特殊字符需要转义
            if (remarks.contains("\"")) {
                remarks = remarks.replace("\"", "'");
            }
            //给model的字段添加swagger注解
            field.addJavaDocLine("@ApiModelProperty(value = \"" + remarks + "\")");
        }
    }
}

```

# 2、In Your Business



```java
@Service
public class UmsAdminServiceImpl implements UmsAdminService {
    /**
     * 创建
     * @param entity
     */
    @Override
    public void create(UmsAdmin entity) {
        umsAdminMapper.insert(entity);
    }

    /**
     * 根据ID删除
     * @param id
     */
    @Override
    public void delete(Long id) {
        umsAdminMapper.deleteByPrimaryKey(id);

    }
    /**
     * 修改
     * @param entity
     */
    @Override
    public void update(UmsAdmin entity) {
        umsAdminMapper.updateByPrimaryKey(entity);

    }

    /***
     * 根据ID查询
     * @param id
     * @return
     */
    @Override
    public UmsAdmin select(Long id) {
        Optional<UmsAdmin> optionalEntity  = umsAdminMapper.selectByPrimaryKey(id);
        return optionalEntity.orElse(null);

    }

    /**
     * 分页查询
     * @param pageNum
     * @param pageSize
     * @return
     */
    @Override
    public List<UmsAdmin> listAll(Integer pageNum, Integer pageSize) {
        PageHelper.startPage(pageNum,pageSize);
        return umsAdminMapper.select(SelectDSLCompleter.allRows());
    }   
}   
```

# 3、SqlBuilder

SqlBuilder是一个非常有用的类，使用它可以灵活地构建SQL语句的条件，一些常用的条件构建方法如下。

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211207081853.jpg)

# 4、SelectStatementProvider

```sql
SELECT
 id,
 username,
 PASSWORD,
 icon,
 email,
 nick_name,
 note,
 create_time,
 login_time,
STATUS 
FROM
 ums_admin 
WHERE
 ( username = 'wangnaixing' AND STATUS IN ( 0, 1 ) ) 
ORDER BY
 create_time DESC;
```

```java
  /**
     * 根据用户名和状态查询
     * @param pageNum
     * @param pageSize
     * @param username
     * @param statusList
     * @return
     */
    @Override
    public List<UmsAdmin> list(Integer pageNum, Integer pageSize, String username, List<Integer> statusList) {
        PageHelper.startPage(pageNum,pageSize);
        SelectStatementProvider statementProvider = SqlBuilder.select(BasicColumn.columnList(
                        UmsAdminDynamicSqlSupport.id,
                        UmsAdminDynamicSqlSupport.username,
                        UmsAdminDynamicSqlSupport.password,
                        UmsAdminDynamicSqlSupport.icon,
                        UmsAdminDynamicSqlSupport.email,
                        UmsAdminDynamicSqlSupport.nickName,
                        UmsAdminDynamicSqlSupport.note,
                        UmsAdminDynamicSqlSupport.createTime,
                        UmsAdminDynamicSqlSupport.loginTime))
                .from(UmsAdminDynamicSqlSupport.umsAdmin)
                .where(UmsAdminDynamicSqlSupport.username, SqlBuilder.isEqualToWhenPresent(username))
                .and(UmsAdminDynamicSqlSupport.status,isIn(statusList))
                .orderBy(UmsAdminDynamicSqlSupport.createTime.descending())
                .build()
                .render(RenderingStrategies.MYBATIS3);

        return umsAdminMapper.selectMany(statementProvider);
   
    }
```

```java
   /**
     * 根据用户名和状态查询 基于lambda
     * @param pageNum
     * @param pageSize
     * @param username
     * @param statusList
     * @return
     */
    @Override
    public List<UmsAdmin> lambdaList(Integer pageNum, Integer pageSize, String username, List<Integer> statusList) {
        PageHelper.startPage(pageNum,pageSize);
        return umsAdminMapper.select(c -> c.where(UmsAdminDynamicSqlSupport.username, isEqualToWhenPresent(username))
                .and(UmsAdminDynamicSqlSupport.status, isIn(statusList))
                .orderBy(UmsAdminDynamicSqlSupport.createTime.descending()));
        
    }
```

# 5、子查询

```sql
SELECT
 * 
FROM
 ums_admin 
WHERE
 id IN ( SELECT admin_id FROM ums_admin_role_relation WHERE role_id = 1 )
```

- 使用Dynamic SQL对应的Java代码实现如下，可以发现SqlBuilder的条件构造方法isIn中还可以嵌套SqlBuilder的查询。

```java
    /**
     * 子查询
     * @param roleId
     * @return
     */
    @Override
    public List<UmsAdmin> subList(Long roleId) {
        SelectStatementProvider statementProvider = SqlBuilder.select(UmsAdminMapper.selectList)
                .from(UmsAdminDynamicSqlSupport.umsAdmin)
                .where(UmsAdminDynamicSqlSupport.id, isIn(
                        SqlBuilder.select(UmsAdminRoleRelationDynamicSqlSupport.adminId)
                                .from(UmsAdminRoleRelationDynamicSqlSupport.umsAdminRoleRelation)
                                .where(UmsAdminRoleRelationDynamicSqlSupport.roleId, isEqualTo(roleId))))
                .build()
                .render(RenderingStrategies.MYBATIS3);

        return umsAdminMapper.selectMany(statementProvider);

    }
```

# 7、GroupBy And  Join

```sql
SELECT
 ur.id AS roleId,
 ur.NAME AS roleName,
 count( ua.id ) AS count 
FROM
 ums_role ur
 LEFT JOIN ums_admin_role_relation uarr ON ur.id = uarr.role_id
 LEFT JOIN ums_admin ua ON uarr.admin_id = ua.id 
GROUP BY
 ur.id;
```

- 先在Dao中添加一个`groupList`方法，然后使用`@Results`注解定义好resultMap；

```java
    public interface UmsAdminDao {

        
    @SelectProvider(type = SqlProviderAdapter.class,method = "select")
    @Results(id = "RoleStatResult",value = {
            @Result(column = "roleId",property = "roleId",jdbcType = JdbcType.BIGINT,id = true),
            @Result(column = "roleName",property = "roleName",jdbcType = JdbcType.VARCHAR),
            @Result(column = "count",property = "count",jdbcType = JdbcType.INTEGER),
    })
    List<RoleStatDto> groupList(SelectStatementProvider selectStatement);
        
  }
```

- 然后在Service中调用`groupList`方法传入StatementProvider即可，对应的Java代码实现如下。

```java
     @Override
    public List<RoleStatDto> groupList() {
        SelectStatementProvider selectStatementProvider = SqlBuilder
                .select(UmsRoleDynamicSqlSupport.id.as("roleId"), UmsRoleDynamicSqlSupport.name.as("roleName"), count(UmsAdminDynamicSqlSupport.id).as("count"))
                .from(UmsRoleDynamicSqlSupport.umsRole)
                .leftJoin(UmsAdminRoleRelationDynamicSqlSupport.umsAdminRoleRelation)
                .on(UmsRoleDynamicSqlSupport.id, equalTo(UmsAdminRoleRelationDynamicSqlSupport.roleId))
                .leftJoin(UmsAdminDynamicSqlSupport.umsAdmin)
                .on(UmsAdminRoleRelationDynamicSqlSupport.adminId,equalTo(UmsAdminDynamicSqlSupport.id))
                .groupBy(UmsRoleDynamicSqlSupport.id)
                .build()
                .render(RenderingStrategies.MYBATIS3);

        return umsAdminDao.groupList(selectStatementProvider);
    }
```

# 8、deleteByCondition

```sql
DELETE 
FROM
 ums_admin 
WHERE
 username = 'andy';
```

- 使用Dynamic SQL对应Java中的实现如下。

```java
  /**
     * 条件删除
     * @param username
     */
    @Override
    public void deleteByUsername(String username) {
        
        DeleteStatementProvider deleteStatementProvider = deleteFrom(UmsAdminDynamicSqlSupport.umsAdmin)
                .where(UmsAdminDynamicSqlSupport.username, isEqualTo(username))
                .build()
                .render(RenderingStrategies.MYBATIS3);
        
        umsAdminMapper.delete(deleteStatementProvider);
        
    }
```

# 9、updateByCondition

```sql
UPDATE ums_admin 
SET STATUS = 1 
WHERE
 id IN ( 1, 2 );
```

- 使用Dynamic SQL对应Java中的实现如下。

```java
 /**
     * 条件修改
     * @param ids
     * @param status
     */
    @Override
    public void updateByIds(List<Long> ids, Integer status) {
        UpdateStatementProvider updateStatementProvider = SqlBuilder.update(UmsAdminDynamicSqlSupport.umsAdmin)
                .set(UmsAdminDynamicSqlSupport.status).equalTo(status)
                .where(UmsAdminDynamicSqlSupport.id, isIn(ids))
                .build()
                .render(RenderingStrategies.MYBATIS3);

        umsAdminMapper.update(updateStatementProvider);

    }
```

# 10、OneToMany

> 使用Dynamic SQL也可以实现一对多查询，只是由于Java注解无法实现循环引用，所以一对多的resultMap只能在mapper.xml来配置，这可能是唯一需要使用mapper.xml的地方。

- 这里以按ID查询后台用户信息（包含对应角色列表）为例，SQL实现如下；

```sql
SELECT
 ua.*,
 ur.id AS role_id,
 ur.NAME AS role_name,
 ur.description AS role_description,
 ur.create_time AS role_create_time,
 ur.STATUS AS role_status,
 ur.sort AS role_sort 
FROM
 ums_admin ua
 LEFT JOIN ums_admin_role_relation uarr ON ua.id = uarr.admin_id
 LEFT JOIN ums_role ur ON uarr.role_id = ur.id 
WHERE
 ua.id = 1
```

- 然后在Dao接口中添加`selectWithRoleList`方法，这里使用`@ResultMap`注解引用mapper.xml中定义的resultMap；

```java
   
public interface UmsAdminDao {
    /**
     * 一对多查询
     * @param selectStatement
     * @return
     */
    @SelectProvider(type = SqlProviderAdapter.class, method = "select")
    @ResultMap("AdminRoleResult")
    AdminRoleDto selectWithRoleList(SelectStatementProvider selectStatement);
    }
```

- 在mapper.xml中添加名称为`AdminRoleResult`的resultMap，这里有个小技巧，可以直接引用在Mapper接口中定义好的resultMap；

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mall.tiny.dao.UmsAdminDao">

    <resultMap id="AdminRoleResult" type="com.wnx.mall.tiny.domain.AdminRoleDto"
               extends="com.wnx.mall.tiny.mbg.mapper.UmsAdminMapper.UmsAdminResult">
        <collection property="roleList" resultMap="com.wnx.mall.tiny.mbg.mapper.UmsRoleMapper.UmsRoleResult" columnPrefix="role_">
        </collection>
    </resultMap>


</mapper>
```

```java

    /**
     * 一对多查询
     * @param id
     * @return
     */
    @Override
    public AdminRoleDto selectWithRoleList(Long id) {
     List<BasicColumn> columnList = new ArrayList<>(CollUtil.toList(UmsAdminMapper.selectList));
        columnList.add(UmsRoleDynamicSqlSupport.id.as("role_id"));
        columnList.add(UmsRoleDynamicSqlSupport.name.as("role_name"));
        columnList.add(UmsRoleDynamicSqlSupport.description.as("role_description"));
        columnList.add(UmsRoleDynamicSqlSupport.createTime.as("role_create_time"));
        columnList.add(UmsRoleDynamicSqlSupport.status.as("role_status"));
        columnList.add(UmsRoleDynamicSqlSupport.sort.as("role_sort"));


        SelectStatementProvider selectStatementProvider = SqlBuilder.select(columnList)
                .from(UmsAdminDynamicSqlSupport.umsAdmin)
                .leftJoin(UmsAdminRoleRelationDynamicSqlSupport.umsAdminRoleRelation)
                .on(UmsAdminDynamicSqlSupport.id, equalTo(UmsAdminRoleRelationDynamicSqlSupport.adminId))
                .leftJoin(UmsRoleDynamicSqlSupport.umsRole)
                .on(UmsAdminRoleRelationDynamicSqlSupport.roleId, equalTo(UmsRoleDynamicSqlSupport.id))
                .where(UmsAdminDynamicSqlSupport.id, isEqualTo(id))
                .build()
                .render(RenderingStrategies.MYBATIS3);
        return umsAdminDao.selectWithRoleList(selectStatementProvider);
        
    }
```

# 11、count

- way 01

```java
    /**
     * 根据用户ID查询用户角色关联数量
     * @param userId
     * @return
     */
    @Override
    public Long findUserRoleCountByUserId(long userId) {
        SelectStatementProvider selectStatement = 
                countColumn(SysUserRoleDynamicSqlSupport.userId)
                .from(SysUserRoleDynamicSqlSupport.sysUserRole)
                .where(SysUserRoleDynamicSqlSupport.userId, isEqualTo(userId))
                .build()
                .render(RenderingStrategies.MYBATIS3);
        return sysUserRoleMapper.count(selectStatement);


    }


```

- way 02

```java
    /**
     * 根据用户ID查询用户角色关联数量
     * @param userId
     * @return
     */
    @Override
    public Long findUserRoleCountByUserId(long userId) {
        SelectStatementProvider selectStatement =
                select(count())
                .from(SysUserRoleDynamicSqlSupport.sysUserRole)
                .where(SysUserRoleDynamicSqlSupport.userId, isEqualTo(userId))
                .build()
                .render(RenderingStrategies.MYBATIS3);


        return sysUserRoleMapper.count(selectStatement);


    }
```

- way 03

```java
    /**
     * 根据用户ID查询用户角色关联数量
     * @param userId
     * @return
     */
    @Override
    public Long findUserRoleCountByUserId(long userId) {
        return sysUserRoleMapper.count(c->c.where(SysUserRoleDynamicSqlSupport.userId, isEqualTo(userId)));
    }
    
```

