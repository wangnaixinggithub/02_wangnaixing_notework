# 1、MySQL-CRUD

```sql
# 创建一个数据库 名称为 mysql_db
create database mysql_db character set utf8;

# 切换当前数据库为 mysql_db
use mysql_db;

# 创建一张 t_student的表
create table if not exists t_student(
    id int(11) auto_increment primary key ,
    stu_name varchar(30),
    stu_age int(18),
    stu_email varchar(30)
);

#创建数据
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'李四',26,'123466789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'王五',26,'123756789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'赵柳',26,'123856789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'刘备',26,'123056789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'关羽',26,'123756789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'张飞',26,'123856789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'张三',26,'123056789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'关鱼叉',26,'133456789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'王小二',26,'143456789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'刘三',26,'123416789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'赵四',26,'123456789@qq.com');
insert into t_student(id, stu_name, stu_age, stu_email) VALUES (null,'王柳',26,'1236556789@qq.com');
```

```


#1、新增学生记录

#2、根据学生ID删除学生记录

#3、修改ID=3的学生的年龄为19岁，邮件为827376239@qq.com

#4、查询ID=2的学生记录

#5、查询全部学生记录


```

