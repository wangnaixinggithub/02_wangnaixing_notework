# Spring-JavaConfig-级联属性-赋值

- Spring的级联属性赋值，前提条件是这个组件已经存在。通过访问成员变量的方式，修改确定引用指向的存储空间中值。

```java
package com.wnx.springmvc.config;

import com.alibaba.druid.pool.DruidDataSource;
import com.wnx.springmvc.model.Dept;
import com.wnx.springmvc.model.Emp;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.*;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;

@Configuration
@ComponentScan(
        basePackages = "com.wnx.springmvc",
        excludeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,classes = Controller.class)})
@EnableAspectJAutoProxy      
@EnableTransactionManagement
public class SpringConfig {
  
    @Bean
    public Dept dept(){
        Dept dept = new Dept();
        dept.setDeptId(1);
        dept.setDeptName("开发一部");
        return dept;
    }
    @Bean
    public Emp emp(Dept dept){
        Emp emp = new Emp();
        emp.setEmpId(1);
        emp.setEmpName("王乃醒");
        emp.setEmpSex("男");

        dept.setDeptId(2);
        dept.setDeptName("开发二部");
        
        emp.setDept(dept);

        return emp;
    }


}

```