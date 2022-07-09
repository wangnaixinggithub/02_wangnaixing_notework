# Array-get-Address-Value

- 根据数组名获取地址引用

```java
#include <iostream>
using namespace std;
int main() {
    
    int arr[10] = { 1,2,3,4,5,6,7,8,9,10 };

    cout << "整个数组所占内存空间为： " << sizeof(arr) << endl;
    cout << "每个元素所占内存空间为： " << sizeof(arr[0]) << endl;
    cout << "数组的元素个数为： " << sizeof(arr) / sizeof(arr[0]) << endl;


    cout << "数组首地址为： " << arr << endl;
    cout << "数组中第一个元素地址为： " << &arr[0] << endl;
    cout << "数组中第二个元素地址为： " << &arr[1] << endl;

    

    return 0;
}

```

- 数组名指向的地址和数组第一个元素地址相同

```properties
整个数组所占内存空间为： 40
每个元素所占内存空间为： 4
数组的元素个数为： 10
数组首地址为： 0xd5b47ff680
数组中第一个元素地址为： 0xd5b47ff680
数组中第二个元素地址为： 0xd5b47ff684
```

