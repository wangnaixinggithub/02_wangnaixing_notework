# Bubble-Sort

```python
def main():
    arr = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
    length = len(arr)
    for i in range(length):
        for j in range(length - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]

    print(arr)


if __name__ == '__main__':
    main()

```

