# MySQL-Constraint-Drop- primaryKey-Constraint 

主键约束的移除，在MySQL中，命令是通用的！

# 1、移除单主键

```sql
# 创建一张单主键的表
create table if not exists t_student(
    id int(11) not null primary key  ,
    name varchar(20) not null
);

#移除表的主键
alter table t_student drop primary key ;
```

# 2、移除多主键

```sql
# 创建一张联合主键
create table if not exists t_student(
    stu_id int(11),
    course_id int(11),
    primary key (stu_id,course_id)
);
# 移除联合主键
alter table t_student drop primary key ;
```

