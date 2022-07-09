# Python-open-read-File

- 打开一个文件，并通过read()方法读取内容

```python
def main():
    f = open('1.txt', encoding="UTF-8")
    print(f.read())
    # 天生我才必有用，千金散尽还复来 仰天大笑出门去，我辈岂是蓬蒿人


if __name__ == '__main__':
    main()

```

- 关闭占用的文件流资源。

```python
def main():
    f = None


try:
    with open('致橡树.txt', 'r', encoding='utf-8') as f:
        print(f.read())
except UnicodeDecodeError:
    print("编码格式错误")
except FileNotFoundError:
    print("此文件不存在")

if __name__ == '__main__':
    main()

```

