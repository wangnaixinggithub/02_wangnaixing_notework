# MySQL-DQL-Use-Cacl-Query

MySQL支持4种运算符

1. 算术运算符
2. 比较运算符
3. 逻辑运算符
4. 位运算符

![image-20220703091127283](005-MySQL-DQL-Use-Cacl-Query.assets/image-20220703091127283.png)

![image-20220703091152360](005-MySQL-DQL-Use-Cacl-Query.assets/image-20220703091152360.png)

![image-20220703091200926](005-MySQL-DQL-Use-Cacl-Query.assets/image-20220703091200926.png)

![image-20220703091211496](005-MySQL-DQL-Use-Cacl-Query.assets/image-20220703091211496.png)



# 1、算数运算符

```sql
#查询出来的产品价格 + 10 块
select pid,price + 10 from product;
#查询出来的产品价格 - 10 块
select pid,price - 10 from product;
#查询出来的产品价格 * 10 块
select pid,price * 10 from product;
#查询出来的产品价格 / 10 块
select pid,price / 10 from product;
#查询出来的产品价格 mod 10 块
select pid,price % 10 from product;
```

# 2、比较运算符

```sql


# 等于
select pid,pname,price,category_id from product where price = 5000;

# 小于和小于等于
select pid,pname,price,category_id from product where price < 5000;
select pid,pname,price,category_id from product where price <= 5000;

# 小于和小于等于
select pid,pname,price,category_id from product where price > 5000;
select pid,pname,price,category_id from product where price >= 5000;

# 安全的等于，两个操作码均为NULL时，其所得值为1；而当一个操作码为NULL时，其所得值为0
select pid,pname,price,category_id from product where price <=> 5000;

#不等于
select pid,pname,price,category_id from product where price != 5000;
select pid,pname,price,category_id from product where price <> 5000;

#判断一个值是否为 NULL
select pid,pname,price,category_id from product where price is null;

#判断一个值是否不为 NULL
select pid,pname,price,category_id from product where price is not null;

#获取最小值 result =  1
select least(1,2,3,4,5,6,7,9,10);

#获取最大值 result = 10
select greatest(1,2,3,4,5,6,7,8,9,10);


# 判断一个值是否落在两个值之间
select * from product where price between 1000 and 200;

#判断一个值是IN列表中的任意一个值
select * from product where pid in (1,2,3,4,5,6);

#判断一个值不是IN列表中的任意一个值
select * from product where pid not in (1,2,3,4,5,6);

# 通配符匹配
# 含有裤的所有商品
select * from product where pname like '%裤%';
# 以裤开头的所有商品
select * from product where pname like '裤%';
# 以裤结尾的所有商品
select * from product where pname like '%裤';
# 第二个字为裤的所有商品
select * from product where pname like '_裤%';
```

# 3、逻辑运算符

```sql
# 逻辑与
select * from product where pname like '裤%' and price >= 150;

# 逻辑或
select * from product where pname like '裤%' or price >= 150;

# 逻辑非
select * from product where pname not like '%裤%' ;
```

# 4、位运算符

```sql
# 位运算符是在二进制数上进行计算的运算符。位运算会先将操作数变成二进制数，进行位运算。然后再将计算结果从二进制数变回十进制数。
select 3&5; -- 位与
select 3|5; -- 位或
select 3^5; -- 位异或
select 3>>1; -- 位左移
select 3<<1; -- 位右移
select ~3;   -- 位取反

```

