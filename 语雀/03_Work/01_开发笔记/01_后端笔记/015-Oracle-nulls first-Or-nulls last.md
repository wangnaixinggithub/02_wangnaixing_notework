# Oracle-`nulls first`-Or-`nulls last `

# 1、nulls first

Nulls first和nulls last是 Oracle Order by支持的语法

nulls first ：Null排在最前面

nulls last : Null排在最后面

```sql
select * from MWT_UD_YXYD_JBXX_JD
         order by  years desc nulls last,
                   quarter desc nulls last,
                   BZSJ desc nulls last,
                   create_date desc;
```

