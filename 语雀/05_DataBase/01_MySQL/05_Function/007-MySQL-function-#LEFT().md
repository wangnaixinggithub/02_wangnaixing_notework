# MySQL-function-#LEFT()

> LEFT() 函数 类似字符串,匹配子串。

# 1、Query All Record By Column

```sql
# 比如我查询全部学生记录，只查询学生名字。
select stu_name from t_student;
```

![image-20220704204552353](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704204552353.png)

# 2、 use  #left(column，1)

```sql
# substring(0,1)
select LEFT(stu_name,1) from t_student;
```

![image-20220704204738854](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704204738854.png)

# 3、use #left(column，2)

```sql
# substring(0,2)
select LEFT(stu_name,2) from t_student;
```

![image-20220704204805684](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704204805684.png)

# 4、use #left(column，3)

```sql
# substring(0,3)
select LEFT(stu_name,3) from t_student;
```

![image-20220704204823868](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704204823868.png)

