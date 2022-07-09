# MySQL-DDL-Create-Table

- 创建一张表

```mysql
create table if not exists t_student(
    stu_id int(11) not null primary key auto_increment,
    stu_name varchar(30),
    stu_gender varchar(20),
    stu_age int(11),
    stu_birth date,
    stu_address varchar(50)
);
```

![image-20220702093114424](012-MySQL-DDL-Create-Table.assets/image-20220702093114424.png)
