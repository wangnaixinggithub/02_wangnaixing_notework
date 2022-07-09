# Oracle-DateType-Handler

# 1、sysdate

Oracle中的`sysdate`关键字可以获取到当前系统时间,格式为yyyy-MM-DD HH24:Mi:SS

```sql
select sysdate from MWT_UD_SDRJHB;
```

![image-20220413091716122](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220413091716122.png)

# 2、tochar()

- 我们可以通过`tochar()`函数把日期类型转化为字符串类型，格式化格式为`YYYY-MM-DD HH24:MI:SS`

```sql
select to_char(sysdate,'YYYY-MM-DD HH24:MI:SS') sysdateStr from MWT_UD_SDRJHB;
```

![image-20220413091956399](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220413091956399.png)

# 3、todate()

- 通过`todate()`函数可以把字符串类型转为日期类型

```sql
select to_date('2022-04-13 09:21:36','YYYY-MM-DD HH24:MI:SS') sysdateDateTime from MWT_UD_SDRJHB;
```

![image-20220413092316124](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220413092316124.png)

# 4、plusDay

- 在sysdate上加1，则是当前系统时间上加1天，加2就是当前系统时间加2天。

```sql
select sysdate,sysdate+1,sysdate+2 from MWT_UD_SDRJHB;
```

![image-20220413092718908](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220413092718908.png)

# 5、trunc()

- 使用截断`trunc()`函数，能获取到时间的年部分，月部分，日部分，时部分，分部分。

```sql
select sysdate,
       trunc(sysdate,'yyyy'),
       trunc(sysdate,'MM'),
       trunc(sysdate,'DD'),
       trunc(sysdate,'HH24'),
       trunc(sysdate,'MI')
from MWT_UD_SDRJHB
```

![image-20220413093622103](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220413093622103.png)

# 6、substr()

- 使用substr() 可进行字符串截取

