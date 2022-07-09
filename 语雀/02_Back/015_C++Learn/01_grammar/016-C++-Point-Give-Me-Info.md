# Point-Give-Me-Info

- 从指针中能获取到什么信息？

```c++
#include <iostream>
using namespace std;


int main() {
    //1.声明一个指针，并指向变量a的内存空间
    int a = 10;
    int *p = &a;
    
    //2.p变量存储的就是a内存空间地址
    cout<<p<<endl;
    
    //3.还能获取到存储值
    cout<<*p<<endl;
}

```

