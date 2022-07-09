# Python-Def

# 1、execute Function

- 无参函数调用

```python
def eat():
    print('我正在吃饭...')


def main():
    eat()


if __name__ == '__main__':
    main()

```

```
我正在吃饭...
```

- 有参函数调用

```python
def study(name, book):
    print(name + '正在学习' + book)


def main():
    study('王乃醒', '<Python入门到跑路>')


if __name__ == '__main__':
    main()

```

```
王乃醒正在学习<Python入门到跑路>
```

- 默认值函数调用

```python
def study(name='王乃醒', book='<Python入门到跑路>'):
    print(name + '正在学习' + book)


def main():
    study()  # 王乃醒正在学习<Python入门到跑路>
    study('黄洁莹')  # 黄洁莹正在学习<Python入门到跑路>
    study(book='<Java入门到跑路>')  # 王乃醒正在学习<Java入门到跑路>
    study('黄洁莹', '<Java入门到跑路>') # 黄洁莹正在学习<Java入门到跑路>


if __name__ == '__main__':
    main()

```

# 2、from mudule2 import *

- Python文件就是一个个模块。可通过from import 导入一个模块的内容。

```python
#mudule.py
from mudule2 import *


def main():
    print(sum_number(1, 2, 3))


if __name__ == '__main__':
    main()

```

```python
#mudule2.py
def sum_number(a, b, c):
    return a + b + c

```

