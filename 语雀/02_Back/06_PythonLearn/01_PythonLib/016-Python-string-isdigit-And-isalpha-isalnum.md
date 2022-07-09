# Python-string-isdigit-And-isalpha-isalnum

- 判断这个字符串是不是数字字符串。

```python
def main():
    str1 = "123"
    # Result => True
    print(str1.isdigit())

    str2 = "abc"
    # Result => False
    print(str2.isdigit())

    str3 = "abc123"
    # Result => False
    print(str3.isdigit())


if __name__ == '__main__':
    main()

```

- 判断这个字符串是不是字母字符串。

```python
def main():
    str1 = "123"
    # Result => False
    print(str1.isalpha())

    str2 = "abc"
    # Result => True
    print(str2.isalpha())

    str3 = "abc123"
    # Result => False
    print(str3.isalpha())


if __name__ == '__main__':
    main()

```

- 判断这个字符串是不是字母和数字构成的字符串,或者是字母字符串，数字字符串，都是True.

```python
def main():
    str1 = "123"
    # Result => True
    print(str1.isalnum())

    str2 = "abc"
    # Result => True
    print(str2.isalnum())

    str3 = "abc123"
    # Result => True
    print(str3.isalnum())

    str4 = "$$$"
    # Result => False
    print(str4.isalnum())


if __name__ == '__main__':
    main()

```

