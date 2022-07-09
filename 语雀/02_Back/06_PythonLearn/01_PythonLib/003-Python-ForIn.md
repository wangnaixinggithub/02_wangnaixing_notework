# Python-ForIn

- 可以遍历列表的每一个元素。

```python
def main():
    list = [1, 2, 3, 4, 5]

    for x in list:
        print(x)


if __name__ == '__main__':
    main()
```

```
1
2
3
4
5
```

- 可以遍历字典，得到每一个Key

```python
def main():
    dict = {
        'id': 1,
        'name': '王乃醒',
        'age': 20
    }

    for key in dict:
        print(key)
       

if __name__ == '__main__':
    main()

```

```
id
1
name
```

- 同理可以遍历集合。得到集合每一项元素的值。

```python
def main():
    set1 = {1, 2, 3, 4, 5}

    for x in set1:
        print(x)


if __name__ == '__main__':
    main()

```

