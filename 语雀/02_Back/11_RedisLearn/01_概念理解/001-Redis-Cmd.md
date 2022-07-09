# Redis Cmd-DbOperation

# 1、config get databases 

- 命令行查看数据库数量databases

```shell
127.0.0.1:6379> config get databases
1) "databases"
2) "16"
```

# 2、select 

- 切换数据库 DB 8

```shell
127.0.0.1:6379> select 8 
OK
127.0.0.1:6379[8]>
```

# 3、dbsize

- 令用于返回当前数据库的 key 的数量

```shell
127.0.0.1:6379[8]> dbsize 
(integer) 0
```

# 4、keys *

```shell
127.0.0.1:6379> keys * # 库中没有键值对的情况
(empty list or set)


127.0.0.1:6379> keys *
1) "age"
2) "name"
```

# 5、flushdb  flushall

- `flushdb`：清空当前数据库中的键值对。
- `flushall`：清空所有数据库的键值对。

# 6、get key

- `exists key`：判断键值对是否存在，根据key.

```python
127.0.0.1:6379> get age # 如果键值对不存在 返回 nil
(nil)
```

# 7、move key dbIndex

- 移动当前键值对到指定数据库,根据key, 数据库索引

```shell
127.0.0.1:6379> move age 1 # 将键值对age 移动到数据库1
(integer) 1 #移动成功
```

# 8、exists key

- 判断键值对是否存在，根据key.

```python
127.0.0.1:6379> EXISTS age # 判断键age是否存在？
(integer) 0 # 0 代表不存在

127.0.0.1:6379> EXISTS name #判断键name是否存在？
(integer) 1 # 1 代表存在
```

# 9、del key

- 删除键值对,根据key.

```shell
127.0.0.1:6379[1]> del age # 删除键值对
(integer) 1 # 删除个数
```

# 10、expire key time

```shell
127.0.0.1:6379> EXPIRE age 15 # 设置键值对的过期时间
```

# 11、ttl key

- 查询键值对的过期时间，根据key.

```shell
127.0.0.1:6379> ttl age 
(integer) 13

127.0.0.1:6379> ttl age
(integer) -2 # -2 表示key过期，-1表示key未设置过期时间

127.0.0.1:6379> get age # 过期的key 会被自动delete
(nil)
```

# 12、type key

```shell
127.0.0.1:6379> type name # 查看value的数据类型
string
```



# 13、APPEND key value

- 向指定的key的value后追加字符串

```shell
127.0.0.1:6379> set msg hello
OK

127.0.0.1:6379> append msg world
(integer) 10

127.0.0.1:6379> get msg
"helloworld"

127.0.0.1:6379>
```

# 14、incr key  decr key

- 将指定键值对的value数值进行 +1 / -1 (仅对于数字)

```shell
127.0.0.1:6379> set age 20
OK

127.0.0.1:6379> incr age
(integer) 21

127.0.0.1:6379> decr age
(integer) 20

127.0.0.1:6379>
```

# 15、incrby key n  decrbykey n

- 将指定键值对的value数值进行+n / -n (仅对于数字)

```shell
127.0.0.1:6379> incrby age 5
(integer) 25

127.0.0.1:6379> decrby age 10
(integer) 15

127.0.0.1:6379>
```

# 16、incrbyfloat key n

- 将指定键值对的value数值进行+n / -n (仅对于数字)  加上一个浮点数

```shell
127.0.0.1:6379> incrbyfloat age 5.5
"20.5"

127.0.0.1:6379>
```

# 17、strlen key

- 获取字符串值的长度

```shell
127.0.0.1:6379> strlen msg
(integer) 10

127.0.0.1:6379> get msg
"helloworld"

127.0.0.1:6379>
```

# 18、getrange key start end

- 按起止位置获取字符串（闭区间，起止位置都取）

```shell
127.0.0.1:6379> get msg
"helloworld"

127.0.0.1:6379> getrange msg 0 4

"hello"
127.0.0.1:6379>
```

# 19、getset key value

- 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。

```shell
127.0.0.1:6379> GETSET msg test
"helloworld"

127.0.0.1:6379> get msg
"test"
```

# 20、setnx key value

- 仅当key不存在时进行set值。

```shell
127.0.0.1:6379> setnx msg hahah #msg entry no exists
(integer) 0

127.0.0.1:6379> setnx msg2 hahah # msg2 is exists
(integer) 1

127.0.0.1:6379> get msg2
"hahah"

127.0.0.1:6379>
```

# 21、setex key seconds value

- set 键值对并设置过期时间，以秒为单位

```shell
127.0.0.1:6379> setex msg3 15 helloredis
OK

127.0.0.1:6379> ttl msg3
(integer) 11
127.0.0.1:6379> ttl msg3
(integer) 9
127.0.0.1:6379> ttl msg3
(integer) 6
127.0.0.1:6379> ttl msg3
(integer) 3
127.0.0.1:6379> ttl msg3
(integer) -2

127.0.0.1:6379> get msg3
(nil)

127.0.0.1:6379>
```

- `psetex key milliseconds value` ，以毫秒为单位。

# 22、mset key1 value1 [key2 value2..]

- 批量设置键值对

```shell
127.0.0.1:6379> mset msg1 java msg2 mysql msg3 redis
OK

127.0.0.1:6379> keys *
1) "msg2"
2) "msg1"
3) "msg3"

127.0.0.1:6379>

```

# 23、MGET key1 [key2..]

- 批量获取多个键值对，根据key.

```shell
127.0.0.1:6379> mget msg1 msg2 msg3
1) "java"
2) "mysql"
3) "redis"

127.0.0.1:6379>
```

# 24、lpush/rpush key value1[value2..]  

- 向集合的左边/右边添加元素

```shell

127.0.0.1:6379> LPUSH mylist k1 # LPUSH mylist=>{1}
(integer) 1

127.0.0.1:6379> LPUSH mylist k2 # LPUSH mylist=>{2,1}
(integer) 2

127.0.0.1:6379> RPUSH mylist k3 # RPUSH mylist=>{2,1,3}
(integer) 3

```

# 25、 lrange key start end  

- 获取起止位置范围内的元素

```shell
127.0.0.1:6379> LRANGE mylist 0 4 # LRANGE 获取起止位置范围内的元素
1) "k2"
2) "k1"
3) "k3"

127.0.0.1:6379> LRANGE mylist 0 2
1) "k2"
2) "k1"
3) "k3"

127.0.0.1:6379> LRANGE mylist 0 1
1) "k2"
2) "k1"

127.0.0.1:6379> LRANGE mylist 0 -1 # 获取全部元素
1) "k2"
2) "k1"
3) "k3"
```

# 26、linsert key before|after elementKey value

- 在指定列表元素的前/后 插入value

```shell
127.0.0.1:6379> LINSERT mylist after k2 ins_key1 # 在k2元素后 插入ins_key1
(integer) 6

127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "ins_key1"
5) "k1"
6) "k3"
```

# 27、llen key

-  查看列表长度

```shell
127.0.0.1:6379> LLEN mylist # 查看mylist的长度
(integer) 6
```

# 28、lindex key index

```shell
127.0.0.1:6379> lindex mylist 3 # 获取下标为3的元素
"ins_key1"
```

# 29、lset key index value

- 通过索引为元素设值

```shell
127.0.0.1:6379> LSET mylist 3 k6 # 将下标3的元素 set值为k6
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "k6"
5) "k1"
6) "k3"
```

# 30、lpop/rpop key

![](001-Redis-Cmd.assets/QQ%E6%88%AA%E5%9B%BE20211117123857.png)

```shell

127.0.0.1:6379> LPOP mylist # 左侧(头部)弹出
"k5"

127.0.0.1:6379> RPOP mylist # 右侧(尾部)弹出
"k3"

```

```shell
127.0.0.1:6379> RPOPLPUSH mylist newlist # 将mylist的最后一个值(k1)弹出，加入到newlist的头部
"k1"

127.0.0.1:6379> LRANGE newlist 0 -1
1) "k1"

127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
3) "k6"
```

# 31、LTRIM



```shell
127.0.0.1:6379> LTRIM mylist 0 1 # 截取mylist中的 0~1部分
OK

127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
```

# 32、LREM

```shell


127.0.0.1:6379> LREM mylist 3 k2 # 从头部开始搜索 至多删除3个 k2
(integer) 3
# 删除后：mylist: k2,k2,k2,k4,k2,k2,k2,k2

127.0.0.1:6379> LREM mylist -2 k2 #从尾部开始搜索 至多删除2个 k2
(integer) 2
# 删除后：mylist: k2,k2,k2,k4,k2,k2

```

