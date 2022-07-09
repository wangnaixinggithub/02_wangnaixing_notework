# Use-MySQL-date_sub(now(), interval 7 day)

## 1、什么意思

> - 意思就是查询出，当前时间，七天前的时间。
>
> - 比如now() 得到当前时间是 `2022-01-15 22:36:43`
>
> - 那么date_sub(now(), interval 7 day)   得到的时间就是 `2022-01-08 22:36:43`
> - 之间刚好差一个星期！

```sql
select date_sub(now(), interval 7 day)  from products;

select now()  from products;
```

## 2、能做什么？

>  做个周统计啥的！

```java
    /**
     * 前台，获取本周热销商品
     *
     */
    List<Map<String, Object>> getWeekHotProduct();
```

```sql
    <!--前台，获取本周热销商品-->
    <select id="getWeekHotProduct" resultType="java.util.Map">
        select products.id as id,products.name as name, products.imgurl as imgurl,SUM(orderitem.buynum) as totalsalnum
                from orderitem,orders,products
                where orderitem.order_id = orders.id
                and products.id = orderitem.product_id
                and orders.paystate=1
                and orders.ordertime > date_sub(now(), interval 7 day )
                group by  products.id,products.name,products.imgurl
                order by totalsalnum desc
                limit 0,2
    </select>
```

```sql
 and orders.ordertime > date_sub(now(), interval 7 day )
```