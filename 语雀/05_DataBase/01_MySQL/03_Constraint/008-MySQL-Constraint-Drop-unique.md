# MySQL-Constraint-Drop-unique

# 1、Way One: Use Name

```sql
drop table t_student;
create table if not exists t_student(
    id int(11) primary key auto_increment,
    stu_name varchar(30) not null ,
    stu_phone varchar(11) not null
);

alter table t_student modify stu_phone varchar(11) not null unique ;
```

```sql
# 唯一约束名默认是等同于 字段名的
alter table t_student drop index stu_phone;
```

![image-20220702221514570](008-MySQL-Constraint-Drop-unique.assets/image-20220702221514570.png)
