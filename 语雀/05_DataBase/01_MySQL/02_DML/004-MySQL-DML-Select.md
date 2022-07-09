# MySQL-DML-Select

# 1、Query All Column 

```sql
select id  nickname,sex from t_user;
```

# 2、 Distinct Query

- 只针对单字段有效，只能查一个字段。

```sql
select  DISTINCT nickname from t_user;
```

# 3、Condition Query

```sql
select * from emp where deptno=1 and sal<3000
```

# 4、Pageable Query

- index = (pageNo - 1) * PageSize

```sql
SELECT * FROM t_user LIMIT 2,2;
```

# 5、Order Query

```sql
SELECT * FROM t_user ORDER BY nickname;
```

# 5、limit Query

```sql
SELECT * FROM t_user LIMIT 2;
```

# 5、Left Join Query

- 左连接，左表记录保留

```sql
select * from emp e left join dept d on e.deptno=d.deptno
```

# 6、Sub Query

```sql
select * from emp where deptno in (select deptno from dept)
```

# 7、union Query

```sql
select deptno from emp union select deptno from dept
```

# 8、Group By Query

- 分组查询，having 对分组查询结果进行过滤。

```sql
/*聚合查询，查询结果安装部门编号分类，再筛选出数据数目大于1的部门。*/
select deptno,count(1) from emp group by deptno having count(1) > 1
```

