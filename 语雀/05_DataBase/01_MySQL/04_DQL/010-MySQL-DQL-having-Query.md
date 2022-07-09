# MySQL-DQL-having-Query

- 分组之后对统计结果进行筛选的话必须使用having
- Øhaving 子句用来从分组的结果中筛选行

```sql
-- 统计各个分类商品的个数,且只显示个数大于4的信息
select category_id,count(1) from product group by category_id having count(1) > 4
```

