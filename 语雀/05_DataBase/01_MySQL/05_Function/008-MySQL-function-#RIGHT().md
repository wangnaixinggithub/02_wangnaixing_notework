# MySQL-function-#RIGHT()

> RIGHT() 函数 类似字符串从后往前截取。匹配是从最后一个字符，往前匹配

# 1、Query All Record By Column

```sql
# 比如我查询全部学生记录，只查询学生名字。
select stu_name from t_student;
```

![image-20220704205623777](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704205623777.png)

# 2、 use  #RIGHT(column，1)

```sql
# str.slice(-1) javascript
select RIGHT(stu_name,1) from t_student;
```

![image-20220704205636807](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704205636807.png)

# 3、use #RIGHT(column，2)

```sql
# str.slice(-2) javascript
select RIGHT(stu_name,2) from t_student;
```

![image-20220704205655715](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704205655715.png)

# 4、use #RIGHT(column，3)

```sql
# str.slice(-3) javascript
select RIGHT(stu_name,3) from t_student;
```

![image-20220704205707876](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704205707876.png)