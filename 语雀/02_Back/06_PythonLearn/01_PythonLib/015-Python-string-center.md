# Python-string-center

- 居中显示，填充大小为50，其余部分用* 取代。

```python
def main():
    str1 = '黄洁莹'
    str2 = str1.center(50, '*')
    # 50
    print(len(str2))
    # ***********************黄洁莹************************
    print(str2)


if __name__ == '__main__':
    main()

```

