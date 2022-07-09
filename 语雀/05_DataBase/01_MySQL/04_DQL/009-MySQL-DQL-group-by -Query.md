# MySQL-DQL-group-by -Query

分组查询是指使用group by字句对查询信息进行分组。

```sql
-- 统计各个分类商品的个数
select category_id ,count(1) from product group by category_id;
```

**![image-20220703100547891](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703100547891.png)**