# MySQL-DQL-limit-Query

# 1、显示前几条

```sql
-- 显示前5条
select * from product limit 5;
```



# 2、分页

```sql
-- 1 - 5
select * from product limit 0,5;
-- 5 - 10
select * from product limit 5,5;
```



![image-20220703111945234](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703111945234.png)