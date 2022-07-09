# Python-sys-stdin-#read()-And-#readLine()

# 1、read()

- 读取用户的输入值，通过用户换行输入 `ctrl+D` 终止输入。打印结果。

```python
import sys


def main():
    str = sys.stdin.read()

    print(type(str))
    print(f'用户输入的值=>{str}')


if __name__ == '__main__':
    main()

```

- 当输入值为`str` 

```python
王乃醒
^D
<class 'str'>
用户输入的值=>王乃醒
```

- 当输入值为`number` 

```python
1
^D
<class 'str'>
用户输入的值=>1
```

- 当用户输入`bool`

```python
true
^D
<class 'str'>
用户输入的值=>true
```

> 可以知道，不管输入什么类型的数，都是输入str.

# 2、readline()

- 读取用户输入的值，通过用户输入`enter`的时候，就终止输入。打印结果。

```python
import sys


def main():
    str = sys.stdin.readline()

    print(type(str))
    print(f'用户输入的值=>{str}')


if __name__ == '__main__':
    main()
```

```python
王乃醒是大帅哥
<class 'str'>
用户输入的值=>王乃醒是大帅哥
```

