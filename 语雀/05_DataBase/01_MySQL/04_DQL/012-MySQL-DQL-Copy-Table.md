# MySQL-DQL-Copy-Table

# 1、Way One

```sql
-- 创建复制表
create table if not exists product_copy(
    pid int(11) auto_increment primary key ,
    pname varchar(20),
    price double,
  category_id varchar(20)
);

-- 把product表 复制给 product_copy
insert into product_copy(pid,pname,price,category_id) select pid,pname,price,category_id from product;


-- 查询复制表结果 SUCCESS
select * from product_copy;
```

# 2、Way Two

```sql

-- 创建复制表
create table if not exists product_copy(
    pid int(11) auto_increment primary key ,
    pname varchar(20),
    price double,
  category_id varchar(20)
);

-- 把product表 复制给 product_copy
insert into product_copy select * from product;


-- 查询复制表结果
select * from product_copy;
```

![image-20220703112834365](012-MySQL-DQL-Copy-Table.assets/image-20220703112834365.png)