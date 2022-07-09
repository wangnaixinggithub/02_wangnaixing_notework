

# Python-Use-Redis

```shell
pip install redis  -i http://pypi.doubanio.com/simple/ --trusted-host pypi.doubanio.com
```

# 1、Operation String

```python
from redis import *


def main():

    rs = StrictRedis()
    
    rs.set('name', 'wangnaixing')  # Set

    print(rs.get('name'))  # GET

    print(rs.type('name'))  # Type is String


if __name__ == '__main__':
    main()

```

# 2、Operation  List

```shell
from redis import *


def main():
    rs = StrictRedis()

    rs.lpush('mylist', 1, 2, 3, 4, 5)  # Set

    print(rs.lrange('mylist', 0, -1))  # All element

    print(rs.lindex('mylist', 0))  # findByIndex

    print(rs.llen('mylist'))  # length

    print(rs.type('mylist'))  # type is list


if __name__ == '__main__':
    main()

```

# 3、Operation Set

```python
from redis import *


def main():
    rs = StrictRedis()

    rs.sadd('my-set', 1, 2, 3, 4, 5)  # Set

    print(rs.scard('my-set'))  # 获取成员数

    print(rs.sismember('my-set', 5))  # isMember?

    print(rs.smembers('my-set'))  # getAllMember

    print(rs.type('my-set'))  # Type is set


if __name__ == '__main__':
    main()

```

# 4、Operation Hash

```java
from redis import *


def main():
    rs = StrictRedis()

    rs.hset(name='user', key='id', value=1)  //SET
    rs.hset(name='user', key='nickname', value='wangnaixing')
    rs.hset(name='user', key='sex', value='Fale')
    rs.hset(name='user', key='tel', value='18154622909')

    rs.hmset(name='user2', mapping={
        'id': 1,
        'nickname': 'wangnaixing',
        'sex': 'Fale',
        'tel': '18154622909'
    })

    print(rs.hgetall('user')) #getAllEntry

    print(rs.hkeys('user'))  # getAllKey

    print(rs.hvals('user'))  # getAllValue

    print(rs.hmget(name='user', keys={'id', 'nickname', 'sex', 'tel'}))  # getValueByKey


if __name__ == '__main__':
    main()

```

