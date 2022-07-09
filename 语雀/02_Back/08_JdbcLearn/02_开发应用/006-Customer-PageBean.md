# Customer-PageBean

|> 如果我们自己写分页，就知道SQL走了两条，假设现在你自己写分页，不用PageHelper这样的工具类，那么我们该怎么写呢？

## 1、封装PageBean对象

> 这里可能会遇到，得到总条数算总页码的情况。

```java
 int totalPage = (int) Math.ceil(totalCount * 1.0 / currentCount);
```

## 2、写底层的两条SQL

> bind这个标签发挥作用！

```sql
    <select id="findBookByName" resultType="com.wnx.mall.tiny.mbg.model.Products">
        <bind name="index" value="(currentPage - 1) * currentCount"/>
        SELECT * FROM products WHERE name LIKE concat('%',#{searchfield},'%')
        LIMIT #{index},#{currentCount}
    </select>
```

