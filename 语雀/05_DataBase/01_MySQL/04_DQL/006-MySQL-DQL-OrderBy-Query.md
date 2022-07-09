# MySQL-DQL-OrderBy-Query

```sql
# 单字段排序
select * from product order by price desc;

#多字段排序
select * from product order by price desc,category_id asc ;

# 去重降序
select price from product order by  price desc ;


```

