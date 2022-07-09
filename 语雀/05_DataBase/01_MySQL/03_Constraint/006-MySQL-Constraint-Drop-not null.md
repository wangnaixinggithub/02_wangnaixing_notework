# MySQL-Constraint-Drop-not null

```sql
create table if not exists t_student(
    id int(11) auto_increment primary key,
    stu_name varchar(20) not null ,
    stu_email varchar(20) not null 
);

#修改列的时候，不加入not null 约束，就等价于移除字段非空约束了！
alter table t_student modify stu_name varchar(20);
alter table t_student modify stu_email varchar(20);
```

