# Python-string-find()

- 字符串获取字符索引，不存在字符返回-1.

```python
def main():
    str = 'wangnaixing'

    # Find Char if exist => Index
    print(str.find('w')) # 0
    print(str.find('a')) # 1
    print(str.find('n')) # 2
    print(str.find('g')) # 3
    
    # No exist => -1
    print(str.find('z')) # -1


if __name__ == '__main__':
    main()

```

