# MyBatis-Handle-OneByOne

```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Dept  implements Serializable {
    private Integer did;
    private String deptName;
}

@Data
@Builder
public class Emp implements Serializable {
    private Integer eid;
    private String empName;
    private String sex;
    private String email;
    /**
     * 多对一关系
     */
    private Dept dept;

}
```

> 期望：查询职工的时候，把职工所属部门的信息也查询出来！

# 1、方式一：级联属性赋值

```java
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.EmpMapper">
    <!--
       自定集结果集映射 让字段和属性一一对应
       Dept Emp必须指定 @AllArgsConstructor @NoArgsConstructor 构造器。

       底层Mybatis会根据无参构造器先new Dept对象，根据反射赋值。
    -->
   <resultMap id="EmpMap" type="Emp">
       <id column="eid" property="eid"/>
       <result column="emp_name" property="empName"/>
       <result column="sex" property="sex"/>
       <result column="email" property="email"/>
       <result column="did" property="dept.did"/>
       <result column="dept_name" property="dept.deptName"/>
   </resultMap>
  <select id="findById" resultMap="EmpMap">
      select e.*,d.dept_name from  tb_emp e  left join tb_dept d on e.did = d.did
      where e.eid = #{id}
  </select>

</mapper>
```

# 2、方式二：association

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.EmpMapper">
    <!--
       javaType： 这个属性的类型
        property：POJO哪一个属性？
        便于底层Mybatis扫描到此标签，知道是这个属性，反射。
    -->
   <resultMap id="EmpMap" type="Emp">
       <id column="eid" property="eid"/>
       <result column="emp_name" property="empName"/>
       <result column="sex" property="sex"/>
       <result column="email" property="email"/>
       <association property="dept" javaType="Dept">
           <result column="did" property="did"/>
           <result column="dept_name" property="deptName"/>
       </association>
   </resultMap>
  <select id="findById" resultMap="EmpMap">
      select e.*,d.dept_name from  tb_emp e  left join tb_dept d on e.did = d.did
      where e.eid = #{id}
  </select>

</mapper>
```

# 3、方式三：分步查询

```java
public interface EmpMapper {

    /**
     * 根据ID查询职工信息
     * @param id ID
     * @return Emp对象模型
     */
    Emp findById(Integer id);
}

public interface DeptMapper {

    /**
     * 根据ID查询部门信息
     * @param id ID
     * @return 部门对象
     */
    Dept findById(Integer id);
}

```

```xml
<!--EmpMapper.xml-->
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.EmpMapper">
    
    <!--
     column:传递哪一个字段，到DeptMapper的findById()方法中！
    -->
   <resultMap id="EmpMap" type="Emp">
       <id column="eid" property="eid"/>
       <result column="emp_name" property="empName"/>
       <result column="sex" property="sex"/>
       <result column="email" property="email"/>
       <association property="dept"
                    column="did"
                    select="com.wnx.mybatis.mapper.DeptMapper.findById">
       </association>
   </resultMap>
  <select id="findById" resultMap="EmpMap">
    select * from tb_emp where eid = #{eid}
  </select>

</mapper>
```

```xml
<!--DeptMapper.xml-->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.DeptMapper">
    <select id="findById" resultType="Dept">
        select * from tb_dept where did = #{did}
    </select>
</mapper>
```

