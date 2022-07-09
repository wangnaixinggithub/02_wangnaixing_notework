# Use-Point-Swap-Two-Number-Value

```c++
#include <iostream>
using namespace std;

/**
 * 交换两个数的值
 * @param p 引用p
 * @param q 引用q
 */
void swap(int *p,int *q){
    int  temp = *p;
    *p = *q;
    *q = temp;

}


int main() {
    int a = 10;
    int b =20;

    swap(&a,&b);
    cout<<a<<endl; //20
    cout<<b<<endl;//10

}

```

