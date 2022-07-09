# MySQL-Constraint-Add-unique

唯一约束（Unique Key）是指所有记录中字段的值不能重复出现

# 1、Way One

```sql
drop table t_student;
create table if not exists t_student(
    id int(11) primary key auto_increment,
    stu_name varchar(30) not null ,
    stu_phone varchar(11) not null unique
);

insert into t_student(id, stu_name, stu_phone) VALUES (null,'王乃醒1','18154622909');
insert into t_student(id, stu_name, stu_phone) VALUES (null,'王乃醒2','18154622910');
insert into t_student(id, stu_name, stu_phone) VALUES (null,'王乃醒3','18154622911');
```

![image-20220702220132537](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702220132537.png)

# 2、Way Two

```sql
create table if not exists t_student(
    id int(11) primary key auto_increment,
    stu_name varchar(30) not null ,
    stu_phone varchar(11) not null
);

alter table t_student  add constraint unique_phoneNumber unique(stu_phone);
insert into t_student(id, stu_name, stu_phone) VALUES (null,'王乃醒1','18154622909');
insert into t_student(id, stu_name, stu_phone) VALUES (null,'王乃醒2','18154622910');
insert into t_student(id, stu_name, stu_phone) VALUES (null,'王乃醒3','18154622911');
```

![image-20220702220120264](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702220120264.png)

# 3、Way Three

```sql
create table if not exists t_student(
    id int(11) primary key auto_increment,
    stu_name varchar(30) not null ,
    stu_phone varchar(11) not null
);

alter table t_student modify stu_phone varchar(11) not null unique ;

```

如果不符合唯一约束，则此数据不得入库，并MySQL报错:

![image-20220702220351983](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702220351983.png)