# C++-Create-Array-Define

- C++提供了三种创建数组的方式。

```java
#include <iostream>
using namespace std;
int main() {
    //1.先声明 再通过下表逐一赋值。
    int score1[5];
    score1[0] = 1;
    score1[1] = 2;
    score1[2] = 3;
    score1[3] = 4;
    score1[4] = 5;
    for (int item:score1) {
        cout<<item<<endl;
    }
    cout<<"------------"<<endl;
    //1
    //2
    //3
    //4
    //5
    //------------
    
    //2.确定数组长度的直接赋值
    int score2[10] = {1,2,3,4,5};

    for (int item:score2) {
        cout<<item<<endl;
    }
    cout<<"------------"<<endl;
    //1
    //2
    //3
    //4
    //5
    //0
    //0
    //0  指定长度的数组其他空间没有被使用的话，用0 填充。
    //0
    //0
    //------------

    //3.不确定数组长度的直接赋值 ,数组长度等价于初始化元素个数。
    int score3[] = {1,2,3,4,5};

    for (int item:score3) {
        cout<<item<<endl;
    }
    cout<<"------------"<<endl;
    //1
    //2
    //3
    //4
    //5
    //------------
}

```

