# MySQL-Constraint-Add-default

MySQL 默认值约束用来指定某列的默认值。

# 1、Way One: In Create Table Define

```java
create table if not exists t_student(
    id int(11) auto_increment primary key ,
    stu_Name varchar(30) not null ,
    stu_phone varchar(11) not null unique ,
    stu_address varchar(20)  default '广州'
);
insert into t_student(id, stu_Name, stu_phone) values (null,'王乃醒','18154622910');
select * from t_student;

```

![image-20220703083927985](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703083927985.png)

# 2、Way Two: Use alter

```sql
create table if not exists  t_student(
    id int(11) auto_increment primary key ,
    stu_name varchar(20) not null ,
    stu_address varchar(20)
);
alter table t_student modify stu_address varchar(20) default '广州';
```

