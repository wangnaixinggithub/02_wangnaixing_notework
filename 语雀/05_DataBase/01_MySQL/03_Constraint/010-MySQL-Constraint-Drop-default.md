# MySQL-Constraint-Drop-default

移除默认值约束

```
alter table <表名> modify column <字段名> <类型> default null; 
```

```sql
alter table t_student modify column stu_address varchar(20) default null;
```

