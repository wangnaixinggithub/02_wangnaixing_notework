# MySQL-DQL-Aggregation-Function

之前我们做的查询都是横向查询，它们都是根据条件一行一行的进行判断，而使用聚合函数查询是纵向查询，它是对一列的值进行计算，然后返回一个单一的值；

另外聚合函数会忽略空值。



![image-20220703094809454](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703094809454.png)



```sql
# 求总数
select count(1) from product;

# 求和
select sum(price) from product;

# 求平均
select avg(price) from product;

# 求最大
select max(price) from product;

# 求最小
select min(price) from product;

```

