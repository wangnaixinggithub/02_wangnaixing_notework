# Mybatis-XmlConfig-lazyLoadingEnabled

> Mybatis使用懒加载策略（延迟加载策略）

# 1、mybatis-config.xml

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    
    <properties resource="jdbc.properties"/>

    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!--
           lazyLoadingEnabled 开启延迟加载的全局配置
        -->
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="true"/>
    </settings>
    <typeAliases>
        <package name="com.wnx.mybatis.pojo"/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>
                <property name="url"
                          value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--指定UserMapper的位置-->
        <package name="com.wnx.mybatis.mapper"/>
    </mappers>
</configuration>
```

# 2、Mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.DeptMapper">

<resultMap id="map01" type="Dept">
    <id property="did" column="did"/>
    <result property="deptName" column="dept_name"/>
    <!--
        在association获取collection中配置   fetchType="lazy" 
    -->

    <collection property="empList"
                ofType="Emp"
                column="did"
                fetchType="lazy" 
                select="com.wnx.mybatis.mapper.EmpMapper.findEmpListByDid"/>
</resultMap>
<select id="findById" resultMap="map01">
    select * from tb_dept  where did = #{did}
</select>

</mapper>
```

