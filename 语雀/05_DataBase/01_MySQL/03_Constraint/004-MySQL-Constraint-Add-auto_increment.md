# MySQL-Constraint-Add-auto_increment

- 在 MySQL 中，当主键定义为自增长后，这个主键的值就不再需要用户输入数据了，而由数据库系统根据定义自动赋值。每增加一条记录，主键会自动以相同的步长进行增长。
- 通过给字段添加 auto_increment 属性来实现主键自增长
- 默认情况下，auto_increment的初始值是 1，每新增一条记录，字段值自动加 1。
- 一个表中只能有一个字段使用 auto_increment约束，且该字段必须有唯一索引，以避免序号重复（即为主键或主键的一部分）。
- auto_increment约束的字段必须具备 NOT NULL 属性。
- auto_increment约束的字段只能是整数类型 TINYINT、SMALLINT、INT、BIGINT 等。
- auto_increment约束字段的最大值受该字段的数据类型约束，如果达到上限，auto_increment就会失效。

# Way One:

```sql
drop table t_student;
create table if not exists t_student(
    id int(11) not null  primary key auto_increment,
    name varchar(30)
) auto_increment =1;
```



# Way Two:

```sql
drop table t_student;
create table if not exists t_student(
    id int(11) not null  primary key auto_increment,
    name varchar(30)
);
alter table t_student auto_increment = 1;
```

