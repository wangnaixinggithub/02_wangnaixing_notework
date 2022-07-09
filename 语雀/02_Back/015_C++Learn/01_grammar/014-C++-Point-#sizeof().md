# Point-#sizeof()

- 指针也是一种数据类型，那么指针在C++内存中占有多少空间字节？

```c++
#include <iostream>
using namespace std;


int main() {

    cout << sizeof(int *) << endl;
    cout << sizeof(char *) << endl;
    cout << sizeof(float *) << endl;
    cout << sizeof(double *) << endl;

    //8
    //8
    //8
    //8

}
```

