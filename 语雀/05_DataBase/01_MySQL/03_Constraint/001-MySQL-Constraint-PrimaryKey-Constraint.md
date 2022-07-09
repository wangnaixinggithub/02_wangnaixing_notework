# MySQL-Constraint-PrimaryKey-Constraint

添加主键约束，其值能唯一地标识表中的每一行,方便在RDBMS中尽快的找到某一行。

当创建主键的约束时，系统默认会在所在的列和列组合上建立对应的唯一索引。

# 1、Way One: In Create Table

```sql
create table if not exists t_student(
    #添加主键约束
    id int(11) not null primary key ,
    name varchar(30)
)
```

![image-20220702203729763](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702203729763.png)

# 2、Way Two: Create Table Later Use Alter

```java
create table if not exists t_student(
    id int(11) not null ,
    name varchar(30) not null
);
alter table t_student add primary key (id);
```

![image-20220702204202090](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702204202090.png)

# 3、Way Three: Use constraint 

```sql
create table if not exists t_student(
    id int(11),
    name varchar(30),
    constraint pk1 primary key (id)
)
```

![image-20220702205334129](001-MySQL-Constraint-PrimaryKey-Constraint.assets/image-20220702205334129.png)
