# MySQL-Constraint-More-PrimaryKey-Constraint

所谓的联合主键，就是这个主键是由一张表中多个字段组成的。

注意：

1. 当主键是由多个字段组成时，不能直接在字段名后面声明主键约束。

2. 一张表只能有一个主键，联合主键也是一个主键

# 1、Way One: In Create Table 

```sql
create table if not exists t_student(
    stu_id int(11),
    course_id int(11),
    primary key (stu_id,course_id)
)
```

![image-20220702205029037](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702205029037.png)

# 2、Way Two: Use Constraint 

```sql
create table if not exists t_student(
    stu_id int(11),
    course_id int(11),
    constraint pk1 primary key (stu_id,course_id)
)
```

# 3、Way Three: Create Table Later Use Alter

```sql
create table if not exists t_student(
    stu_id int(11),
    course_id int(11)
);
alter table t_student add primary key (stu_id,course_id);
```

