# C++-Two-Dimensional-Array-get-Address-Value

```c++
#include <iostream>
using namespace std;
int main() {
    int arr[2][3] = {
            {1,2,3},
            {4,5,6}
    };

    cout << "二维数组大小： " << sizeof(arr) << endl;
    cout << "二维数组一行大小： " << sizeof(arr[0]) << endl;
    cout << "二维数组元素大小： " << sizeof(arr[0][0]) << endl;

    cout << "二维数组行数： " << sizeof(arr) / sizeof(arr[0]) << endl;
    cout << "二维数组列数： " << sizeof(arr[0]) / sizeof(arr[0][0]) << endl;


    cout << "二维数组首地址：" << arr << endl;
    cout << "二维数组第一行地址：" << arr[0] << endl;
    cout << "二维数组第二行地址：" << arr[1] << endl;

    cout << "二维数组第一个元素地址：" << &arr[0][0] << endl;
    cout << "二维数组第二个元素地址：" << &arr[0][1] << endl;
    cout << "二维数组第三个个元素地址：" << &arr[0][2] << endl;

    cout<<&arr[1][0]<<endl;
    cout<<&arr[1][1]<<endl;
    cout<<&arr[1][2]<<endl;
    return 0;
    
}
//二维数组大小： 24
//二维数组一行大小： 12
//二维数组元素大小： 4
//二维数组行数： 2
//二维数组列数： 3
//二维数组首地址：0x700dfffd20
//二维数组第一行地址：0x700dfffd20
//二维数组第二行地址：0x700dfffd2c
//二维数组第一个元素地址：0x700dfffd20
//二维数组第二个元素地址：0x700dfffd24
//二维数组第三个个元素地址：0x700dfffd28
//0x700dfffd2c
//0x700dfffd30
//0x700dfffd34
```

