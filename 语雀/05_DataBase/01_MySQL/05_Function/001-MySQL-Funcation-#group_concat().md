# MySQL-Funcation-#group_concat()

`group_concat()`函数首先根据`group by`指定的列进行分组，并且用分隔符分隔，将同一个分组中的值连接起来，返回一个字符串结果。

![image-20220703114510464](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703114510464.png)

说明：

　　（1）使用distinct可以排除重复值；

　　（2）如果需要对结果中的值进行排序，可以使用order by子句；

　　（3）separator是一个字符串值，默认为逗号。

# 1、Data Create

```sql
create table emp(
    emp_id int primary key auto_increment comment '编号',
    emp_name char(20) not null default '' comment '姓名',
    salary decimal(10,2) not null default 0 comment '工资',
    department char(20) not null default '' comment '部门'
);

insert into emp(emp_name,salary,department)
values('张晶晶',5000,'财务部'),('王飞飞',5800,'财务部'),('赵刚',6200,'财务部'),('刘小贝',5700,'人事部'),
('王大鹏',6700,'人事部'),('张小斐',5200,'人事部'),('刘云云',7500,'销售部'),('刘云鹏',7200,'销售部'),
('刘云鹏',7800,'销售部');

```

# 2、Use It





- 默认情况为，先把名字都查询出来，然后通过逗号连接。

```sql
select group_concat(emp_name) from emp;
```

![image-20220703114647864](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703114647864.png)

- 我们可以使用`distinct` 去重

```sql
select group_concat( distinct emp_name) from emp;
```

![image-20220703114826479](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703114826479.png)

- 我们可以使用`separator` 修改分隔符

```sql
select group_concat( distinct emp_name separator '-') from emp;
```

![image-20220703115002739](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703115002739.png)



- 我们可以用使用`order by` 对数据进行排序，之后再组合。

```sql
select group_concat( distinct emp_name order by salary desc ) from emp;
```

![image-20220703115435657](013-MySQL-Funcation-%23group_concat().assets/image-20220703115435657.png)
