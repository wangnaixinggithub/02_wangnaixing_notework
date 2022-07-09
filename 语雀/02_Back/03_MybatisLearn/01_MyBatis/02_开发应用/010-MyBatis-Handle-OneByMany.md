# Mybatis处理关系型数据库一对多关系



```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Emp implements Serializable {
    private Integer eid;
    private String empName;
    private String sex;
    private String email;
}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Dept  implements Serializable {
    private Integer did;
    private String deptName;
    /**
     * 一对多
     */
    private List<Emp> empList;
}
```

# 1、方式一：collection

```java
public interface DeptMapper {

    /**
     * 根据ID查询部门信息
     * @param id ID
     * @return 部门对象
     */
    Dept findById(Integer id);
}
```

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.DeptMapper">

<resultMap id="map01" type="Dept">
    <id property="did" column="did"/>
    <result property="deptName" column="dept_name"/>
    <collection property="empList" ofType="Emp">
        <id property="eid" column="eid"/>
        <result column="emp_name" property="empName"/>
        <result column="sex" property="sex"/>
        <result column="email" property="email"/>
    </collection>
</resultMap>
<select id="findById" resultMap="map01">
    select * from tb_dept d
    left join tb_emp e on d.did = e.did
    where d.did = #{did}
</select>


</mapper>
```

# 2、方式二：分步查询

```java
public interface DeptMapper {
    Dept findById(Integer id);
}

public interface EmpMapper {
   List<Emp> findEmpListByDid(Integer did);
}

```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.DeptMapper">

    <resultMap id="map01" type="Dept">
        <id property="did" column="did"/>
        <result property="deptName" column="dept_name"/>
        <collection property="empList"
                    ofType="Emp"
                    column="did"
                    select="com.wnx.mybatis.mapper.EmpMapper.findEmpListByDid"/>
    </resultMap>
    <select id="findById" resultMap="map01">
        select * from tb_dept  where did = #{did}
    </select>

</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.EmpMapper">
  
    <select id="findEmpListByDid" resultType="Emp">
        select * from tb_emp where did = #{did}
    </select>
    
</mapper>
```

