# Input-Two-Value-Do-Sum-Print



数据范围：数据组数满足 ![img](https://www.nowcoder.com/equation?tex=1%20%5Cle%20t%20%5Cle%20100%20%5C) ，数据大小满足 ![img](https://www.nowcoder.com/equation?tex=1%20%5Cle%20a%2Cb%20%5Cle%201000%20%5C)



##### **输入描述**



```javascript
输入第一行包括一个数据组数t(1 <= t <= 100)
接下来每行包括两个正整数a,b(1 <= a, b <= 1000)
```



##### **输出描述:**



```javascript
输出a+b的结果
```



##### **输入例子1:**



```javascript
2
1 5
10 20
```



##### **输出例子1:**



```javascript
6
30
```



```java
import sys


def main():
    while True:
        line = sys.stdin.readline()
        if not line:
            break

        number_list = line.split()
        print(int(number_list[0]) + int(number_list[1]))


if __name__ == '__main__':
    main()
```