# Redis-Value-Datatype

# 1、String

- 值为String类型

```shell
127.0.0.1:6379> set name wangnaixing #Set
OK

127.0.0.1:6379> get name #	GET
"wangnaixing"

127.0.0.1:6379> type name #Type is String
string

127.0.0.1:6379>
```

# 2、List

- 值为List类型

```shell
127.0.0.1:6379> lpush mylist 1 2 3 4 5 #Set
(integer) 5

127.0.0.1:6379> lrange mylist 0 -1 # All element
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"

127.0.0.1:6379> lindex mylist 0 #findByIndex
"5"

127.0.0.1:6379> llen mylist # length
(integer) 5
127.0.0.1:6379> type mylist # type is List
list

127.0.0.1:6379>
```

```shell
127.0.0.1:6379> lset mylist 4 6 #setValueByIndex
OK

127.0.0.1:6379> lpop mylist # lpop 左弹
"5"

127.0.0.1:6379> rpop mylist # rpop 右弹
"6"

127.0.0.1:6379> ltrim mylist 0 1 #trim 截取
OK

127.0.0.1:6379> lrem mylist 1 2 #lrem 移除
(integer) 0
127.0.0.1:6379> lrem mylist 1 3
(integer) 1

127.0.0.1:6379>
```

# 3、Set

- 值为Set集合

```shell
127.0.0.1:6379> sadd myset 1 2 3 4 5 #Set
(integer) 5


127.0.0.1:6379> scard myset #获取成员数
(integer) 5

127.0.0.1:6379> sismember myset 5 # isMember?
(integer) 1

127.0.0.1:6379> smembers myset # getAllMember
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"

127.0.0.1:6379> type myset # Type is Set
set

127.0.0.1:6379>
```

# 4、Hash

- 值为Hash,特别适合存储对象

```shell
127.0.0.1:6379> hmset user id 1 nickname wangnaixing sex Fale tel 18154622909 #Set
OK

127.0.0.1:6379> hgetall user #getAllEntry
1) "id"
2) "1"
3) "nickname"
4) "wangnaixing"
5) "sex"
6) "Fale"
7) "tel"
8) "18154622909"

127.0.0.1:6379> hkeys user #getAllkey
1) "id"
2) "nickname"
3) "sex"
4) "tel"


127.0.0.1:6379> hvals user #getAllValue
1) "1"
2) "wangnaixing"
3) "Fale"
4) "18154622909"
127.0.0.1:6379>

127.0.0.1:6379> hmget user id nickname sex tel #getValueByKey
1) "1"
2) "wangnaixing"
3) "Fale"
4) "18154622909"


```

