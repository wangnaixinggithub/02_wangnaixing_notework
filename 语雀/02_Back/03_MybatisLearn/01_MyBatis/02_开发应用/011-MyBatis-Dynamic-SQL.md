# Mybatis动态SQL

# 1、EmpMapper

```java
package com.wnx.mybatis.mapper;

import com.wnx.mybatis.pojo.Emp;

import java.util.List;

/**
 * 动态SQL,就是根据Mybatis的标签，来组装想要的SQL进行数据库查询
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/1 21:48
 */
public interface EmpMapper {

   /**
    * 多条件查询
    * @param emp
    * @return
    */
   List<Emp> findByManyCondition(Emp emp);

   /**
    * 选择查询，在成立的条件中只去执行第一个满足的！
    * @param emp
    * @return
    */
   List<Emp> findByChoose(Emp emp);

   /**
    * 批量添加
    * @param empList Model构成的集合
    */
   void  insertBatch(List<Emp> empList);

   /**
    * 批量删除
    * @param eids 职工ID们
    */
   void deleteByBatchWay01(Integer[] eids);
   /**
    * 批量删除
    * @param eids 职工ID们
    */
   void deleteByBatchWay02(Integer[] eids);


}

```

# 2、EmpMapper.xml

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatis.mapper.EmpMapper">
  
  <!--select > foreach -->  
<select id="selectByIds" resultType="com.wnx.mybatis.model.SysUser">
    SELECT
    id,user_name,user_password,user_email,user_info,head_img,create_time
    FROM sys_user
    WHERE id IN
<foreach collection="list" open="(" separator="," close=")" item="id" index="i">
      #{id}
 </foreach>
</select>
  
<!--select bind -->
<select id="selectByUsernameLikeAndUserEmail2" resultType="com.wnx.mybatis.model.SysUser">
    SELECT
    id,user_name,user_password,user_email,user_info,head_img,create_time
    FROM sys_user
    <where>
      <if test="userName!=null and userName != ''">
        <bind name="userName" value="'%'+ #{userName} + '%'"/>
        AND user_name LIKE #{userName}
      </if>
      <if test="userEmail!=null and userEmail != ''">
        AND user_email = #{userEmail}
      </if>
    </where>
</select>  
  
<!--insert foreach -->  
<insert id="insertList" useGeneratedKeys="true" keyProperty="id" keyColumn="id">
        INSERT INTO sys_user(
            user_name,
            user_password,
            user_email,
            user_info,
            head_img,
            create_time
        )
        VALUES
<foreach collection="list" index="i" item="sysUser"  separator=",">
           (
           #{sysUser.userName},
           #{sysUser.userPassword},
           #{sysUser.userEmail},
           #{sysUser.userInfo},
           #{sysUser.headImg,jdbcType=BLOB},
           #{sysUser.createTime,jdbcType=TIMESTAMP}
           )
     </foreach>
</insert> 
  
  
 <set id="updateSelect">
    UPDATE sys_user
         <set>
             <if test="userName != null and userName != ''">
                 user_name = #{userName},
             </if>
             <if test="userPassword != null and userPassword != ''">
                 user_password = #{userPassword},
             </if>
             <if test="userEmail != null and userEmail != ''">
                 user_email = #{userEmail},
             </if>
             <if test="userInfo != null and userInfo != ''">
                 user_info = #{userInfo},
             </if>
             <if test="headImg != null and headImg != ''">
                 head_img = #{headImg,jdbcType=BLOB},
             </if>
             <if test="crateTime != null and crateTime != ''">
                 create_time = #{crateTime,jdbcType=TIMESTAMP}
             </if>
         </set>
        WHERE id = #{id}
  </set>
  

   
</mapper>       
```

```xml
 <if test="list!=null and list.size()>0">

</if>
```

