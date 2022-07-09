# MySQL-Constraint-Add-not null

添加字段非空约束

# 1、Way One

在定义表的时候，就添加非空约束

```sql
create table if not exists t_student(
    id int(11) primary key auto_increment not null,
    stu_name varchar(30) not null ,
    stu_age varchar(30) not null ,
    stu_email varchar(30) not null
)
```

# 2、Way One

定义完表了之后，通过修改表定义添加进非空约束。

```sql
create table if not exists t_student(
    id int(11) auto_increment primary key ,
    stu_name varchar(30),
    stu_age varchar(20),
    stu_email varchar(30)
);
alter table t_student modify stu_name varchar(30) not null;
alter table t_student modify stu_age varchar(20) not null ;
alter table t_student modify  stu_email varchar(30) not null ;

```

