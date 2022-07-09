# MyBatisPlus-Page

# 1、使用分页

```java
    //API玩法
    @Test
    public void test(){
        Page<User> page = new Page<>(1,3);
         userMapper.selectPage(page, null);
        //获取分页数据
        List<User> list = page.getRecords();
        list.forEach(System.out::println);
        System.out.println("当前页："+page.getCurrent());
        System.out.println("每页显示的条数："+page.getSize());
        System.out.println("总记录数："+page.getTotal());
        System.out.println("总页数："+page.getPages());
        System.out.println("是否有上一页："+page.hasPrevious());
        System.out.println("是否有下一页："+page.hasNext());
        
    }

```

# 2、XML分页查询

```java
public interface UserMapper extends BaseMapper<User> {

    /**
     * 根据年龄查询用户列表，分页显示
     * @param page 分页对象,xml中可以从里面进行取值,传递参数 Page 即自动分页,必须放在第一位
     * @param age 年龄
     * @return
     */
    IPage<User> pageByXml(@Param("page") Page<User> page, @Param("age") Integer age);

}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wnx.mybatisplus.mapper.UserMapper">

    <sql id="BaseColumns">uid,user_name,age,email</sql>
    <!--1.让查询出来的结果带上分页条件-->
    <select id="pageByXml" resultType="User">
        SELECT <include refid="BaseColumns"/> FROM t_user WHERE age > #{age}
    </select>
</mapper>
```

# 3、Example分页查询

```java
 @GetMapping("/findPageByCondition")
    public CommonResult findPageByCondition(){
        Page<TblEmployee> page = new Page<>(1,2);
        QueryWrapper<TblEmployee> example = new QueryWrapper<>();
        example.between("age",20,50);
        example.eq("gender",1);
        employeeService.page(page,example);
        return CommonResult.success(page);
    }
```

