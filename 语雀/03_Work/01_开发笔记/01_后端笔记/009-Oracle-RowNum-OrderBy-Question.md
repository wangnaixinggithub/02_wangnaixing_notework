# Oracle-RowNum-OrderBy-Question

# 1、RowNum->标识记录行。Create记录,就有RowNum字段！注意乱序问题。

- RowNum是Orcle记录行记录数的一个的数。比如我查询MWT_UD_SDRJHB表全部字段以及行数ROWNUM。

```sql
select ROWNUM R,t.* from MWT_UD_SDRJHB t;
```

- 可以看到行数从1开始排列到20.Oracle有一个规定，新生成的数据总是排列在前面，我在昨天创建了两条数据，今天创建了额外的十八条数据，发现新创建的十八条数据排列在昨天的那两条的前面。

<img src="C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220413085550747.png" alt="image-20220413085550747" style="zoom: 50%;" />

- 如果我附加了查询条件，这个行数则是在加了查询条件之前，这个`ROW_NUM`已经存在。从而查询出来的`ROW_NUM显示出乱序`。

```sql
select ROWNUM R,t.CREATE_TIME from MWT_UD_SDRJHB t order by CREATE_TIME desc
```

<img src="C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220413090456179.png" alt="image-20220413090456179" style="zoom: 50%;" />

2、子查询，避免乱序

```sql
select ROWNUM r, t1.CREATE_TIME from (select ROWNUM ,t.* from MWT_UD_SDRJHB t order by CREATE_TIME desc) t1;
```

<img src="C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220413091005517.png" alt="image-20220413091005517" style="zoom:50%;" />