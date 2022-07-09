# MySQL-DQL-Where

```sql

# 1、给 t_student 表添加一列 入学时间 并设定入学时间类型为 date
alter table t_student add column enter_date date;

# 2、让所有的学生入学时间变为 2022年7月4日
update t_student set enter_date = '2022-07-04';

# 3、将 ID 为 1  3 5 7 9 的学生 入学时间修改 2022年3月4号
update t_student set enter_date = '2022-03-04' where id in (1,3,5,7,9);





# 1、查询学生年龄在 18-20岁范围的内所有学生

# 2、查询学生年龄大于 20 岁的所有学生

# 3、查询姓王的所有学生

# 4、查询学生姓氏中第二个字为小的学生名单
```

