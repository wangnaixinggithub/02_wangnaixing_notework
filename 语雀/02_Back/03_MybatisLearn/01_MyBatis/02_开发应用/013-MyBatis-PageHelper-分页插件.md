# MyBatis-PageHelper-分页插件

## 1、在Mybatis中整合

```xml
        <!--分页插件-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.2.0</version>
        </dependency>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="jdbc.properties"/>
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="false"/>
        <setting name="cacheEnabled" value="true"/>
    </settings>
    <typeAliases>
        <package name="com.wnx.mybatis.pojo"/>
    </typeAliases>
    <plugins>
        <!--设置分页插件-->
        <plugin interceptor="com.github.pagehelper.PageInterceptor"/>
    </plugins>
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
        <package name="com.wnx.mybatis.mapper"/>
    </mappers>
</configuration>
```

## 2、开启分页，把查询集合进行构建分页对象

```java
     @Test
    public void findByPage(){
		PageHelper.startPage(1,10);
        List<Emp> empList =  empMapper.selectByExample(null);
        PageInfo<Emp> pageInfo = new PageInfo<>(empList);
    }
```

## 3、分页对象包含许多分页信息

```java
    @Test
    public void findByPage(){
        PageHelper.startPage(1,10);
        List<Emp> empList =  empMapper.selectByExample(null);
        PageInfo<Emp> pageInfo = new PageInfo<>(empList);

        System.out.println("---------分页相关数据----------");
        int totalPages = pageInfo.getPages();
        int pageNum = pageInfo.getPageNum();
        int pageSize = pageInfo.getPageSize();
        long total = pageInfo.getTotal();
        List<Emp> list = pageInfo.getList();
        int[] navNumber = pageInfo.getNavigatepageNums();
        boolean isFirstPage = pageInfo.isIsFirstPage();
        boolean isLastPage = pageInfo.isIsLastPage();
        int prePage = pageInfo.getPrePage();
        int nextPage = pageInfo.getNextPage();
        boolean hasNextPage = pageInfo.isHasNextPage();
        boolean hasPreviousPage = pageInfo.isHasPreviousPage();
        System.out.println("总记录数"+total);
        System.out.println("当前页"+pageNum);
        System.out.println("每页大小"+pageSize);
        System.out.println("总页码"+totalPages);
        System.out.println("导航分页的页码"+ Arrays.toString(navNumber));
        System.out.println("判断当前页是第一页吗？"+isFirstPage);
        System.out.println("判断当前页是最后一页吗？"+isLastPage);
        System.out.println("判断当前页还有下一页吗？"+hasNextPage);
        System.out.println("判断当前页还有上一页吗？"+hasPreviousPage);
        System.out.println("当前页的上一页的页码"+prePage);
        System.out.println("当前页的下一页的页码"+nextPage);

        System.out.println("==分页数据==");
        list.forEach(System.out::println);

    }
```

