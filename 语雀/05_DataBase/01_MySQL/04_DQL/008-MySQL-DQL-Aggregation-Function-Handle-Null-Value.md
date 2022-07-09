# MySQL-DQL-Aggregation-Function-Handle-Null-Value

# 1、Data Create 

测试空值对聚合函数的影响，插入c2列一条为空的数据。

```sql
-- 创建表
create table test_null( 
 c1 varchar(20), 
 c2 int 
);

-- 插入数据
insert into test_null values('aaa',3);
insert into test_null values('bbb',3);
insert into test_null values('ccc',null);
insert into test_null values('ddd',6);


```

# 2、Invoke

```sql
-- 测试
select count(*), count(1), count(c2) from test_null;
select sum(c2),max(c2),min(c2),avg(c2) from test_null;
```

- `count()` 如果使用字段的话，null值的列不被统计。

![image-20220703095651298](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703095651298.png)

- 使用其他函数，如`sum()` `avg()` `max()` `min()` Null值会被直接忽略。

​	![image-20220703095854608](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703095854608.png)