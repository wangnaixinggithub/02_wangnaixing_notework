# SpringBootConfig-PaginationInterceptor

- 在SpringBoot中配置使用分页。

```java
package com.wnx.mybatisplusdemo.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import com.baomidou.mybatisplus.extension.plugins.pagination.optimize.JsqlParserCountOptimize;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan("com.wnx.mybatisplusdemo.modules.*.mapper")
public class MyBatisConfig {
    

    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
        return paginationInterceptor;
    }


}

```

- 使用

```java
   @GetMapping("/page")
    public CommonResult page(){
        Page<TblEmployee> page = new Page<>(1,2);
        employeeService.page(page, null);
        List<TblEmployee> employeeList = page.getRecords();
        employeeList.forEach(System.out::println);
        System.out.println("获取总条数:" + page.getTotal());
        System.out.println("获取当前页码:" + page.getCurrent());
        System.out.println("获取总页码:" + page.getPages());
        System.out.println("获取每页显示的数据条数:" + page.getSize());
        System.out.println("是否有上一页:" + page.hasPrevious());
        System.out.println("是否有下一页:" + page.hasNext());
        return CommonResult.success(page);
    }
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

