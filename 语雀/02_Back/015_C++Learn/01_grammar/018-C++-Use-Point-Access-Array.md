# Use-Point-Access-Array

```c++
#include <iostream>
using namespace std;


int main() {
    int arr[5] = {10,20,30,40,50};
    //1.指针指向数组
    int *p = arr;

    //2.默认是指向第一个元素的值
    cout<<*p<<endl;

    //指针后移一个，指向第二个元素
    p++;
    cout<<*p<<endl;

    //指针后移一个，指向第三个元素
    p++;
    cout<<*p<<endl;

    p++;
    cout<<*p<<endl;

    p++;
    cout<<*p<<endl;
    

}

```

```c++
#include <iostream>
using namespace std;


int main() {
    int arr[5] = {10,20,30,40,50};
    int *p = arr;
    int length = sizeof arr/sizeof arr[0];

    for (int i = 0; i < length; ++i) {
        cout<<*p<<endl;
        p++;
    }

}

//10
//20
//30
//40
//50
```

