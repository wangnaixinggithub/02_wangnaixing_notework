# C++-#cin()

- 可以读取控制台用户的输入，赋值给对应类型变量。

```c++
#include <iostream>
#include<string>
using namespace std;
int main() {
    short number1;
    int  number2;
    long number3;
    long long number4;

    float f1;
    double d1;


    bool b1;

    char c1;

    string str1;


    cin >>number1;
    cin>>number2;
    cin>>number3;
    cin>>number4;

    cin>>f1;
    cin>>d1; //只能输入 0 或 1 => bool 底层就是用 0 表示false 1表示ture

    cin>>b1;
    cin>>c1;
    cin>>str1;

    cout<<number1;
    cout<<number2;
    cout<<number3;
    cout<<number4;

    cout<<f1;
    cout<<d1;

    cout<<c1;
    cout<<b1;

    cout<<str1;
}


```

