# MySQL-DML-truncate

truncate  作用是删除表，再根据定义创建出表。

我下方先使用`truncate` 清空了数据，再执行新增。ID自增会重新从1开始。

如果是`delete` 清空了数据，再执行新增，ID会接着使用上次自增的到数字。

```sql
truncate t_student;
insert into t_student(id, stu_name, stu_age) values (null,'王乃醒',30);
```

