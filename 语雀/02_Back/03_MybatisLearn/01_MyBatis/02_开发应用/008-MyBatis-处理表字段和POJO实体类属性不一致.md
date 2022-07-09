# MyBatis-处理表名和实体名不一致

# 1、设置表别名

```xml
    <!--
        MYSQL设置字段别名，让表字段和POJO属性名保持一致
    -->
  <select id="findById" resultType="Emp">
      select eid,emp_name empName,age,sex,email,d_id from  tb_emp where eid = #{eid}
  </select>
```

# 2、或者开启全局配置

```xml
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
```

# 3、或者手动设置ResultMap

```xml
    <!--
       自定集结果集映射 让字段和属性一一对应
    -->
   <resultMap id="EmpMap" type="Emp">
       <id column="eid" property="eid"/>
       <result column="emp_name" property="empName"/>
       <result column="sex" property="sex"/>
       <result column="email" property="email"/>
   </resultMap>
  <select id="findById" resultMap="EmpMap">
      select * from  tb_emp where eid = #{eid}
  </select>
```

