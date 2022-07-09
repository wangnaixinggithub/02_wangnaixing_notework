# Python- in-Operation

- in 操作符，可以判断字符串是否出现在str中。

```python
def main():
    str = 'wang'
    # All True => It's In
    print('w' in str)
    print('a' in str)
    print('n' in str)
    print('g' in str)

    # All False  => Not In
    print('x' in str)
    print('y' in str)
    print('z' in str)


if __name__ == '__main__':
    main()

```

- in 操作符，可以判断列表元素，是否出现在list中。

```python
def main():
    list = {1, 2, 3, 4}
    # All True => It's In
    print(1 in list)
    print(2 in list)
    print(3 in list)
    print(4 in list)

    # All False  => Not In
    print(5 in list)
    print(6 in list)
    print(7 in list)


if __name__ == '__main__':
    main()

```

