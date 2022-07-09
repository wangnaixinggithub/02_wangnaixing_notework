# Python-str-#split()

可以把字符串，拆分成字符串数组。

# 1、split()

- 默认不传参数，把空格符作为分隔符进行字符串的拆分工作。

```python

def main():
    str = 'aa bb cc'

    list = str.split()

    print(type(list))

    print(list)


if __name__ == '__main__':
    main()

```

```python
<class 'list'>
['aa', 'bb', 'cc']
```

# 2、str.split(sep=',')

- 同理我们可以传递分隔符参数，进行自定义分隔字符串。比如采用`,`作为分隔字符串。

```python

def main():
    str = 'aa,bb,cc'

    list = str.split(',')

    print(type(list))

    print(list)


if __name__ == '__main__':
    main()

```

