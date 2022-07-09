# Python-len()

- #len()， 可以计算长度。包括string,list ,dict,set,tuple

```python
def main():
    str1 = '王乃醒'
    print(len(str1))  # 3

    list1 = [1, 2, 3, 4, 5]
    print(len(list1))  # 5

    dict1 = {
        "id": 1,
        "name": "王乃醒"
    }
    print(len(dict1))  # 2

    set1 = {1, 2, 3, 4, 5}
    print(len(set1))  # 5

    tuple1 = (1, 2, 3, 4)
    print(len(tuple1))  # 4


if __name__ == '__main__':
    main()

```

