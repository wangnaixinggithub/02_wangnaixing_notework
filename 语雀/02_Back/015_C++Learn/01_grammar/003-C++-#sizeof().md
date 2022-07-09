# C++-#sizeof()

- 计算数据类型占有内存字节数。

```c++
#include <iostream>
using namespace std;
int main() {

    cout<<sizeof (short)<<endl; //2
    
    cout<<sizeof (int)<<endl; //4
    
    cout<<sizeof (long)<<endl;//4
    
    cout<<sizeof (long long )<<endl;//8

    cout<<sizeof (float)<<endl;//4
    
    cout<<sizeof (double)<<endl;//8

    cout<<sizeof (bool )<<endl;//1

    cout<<sizeof (char )<<endl;//1

    cout<<sizeof (string)<<endl;//32

    return 0;
}

```

- 数据类型规定了，变量占用了多大的存储空间。所以`#sizeof()` 也可以作用于变量。

```c++
#include <iostream>
using namespace std;
int main() {
    int num1 = 10;
    cout<<sizeof num1 <<endl; //4
    return 0;
}
```

- 可以通过#sizeof(),计算整个数组占用空间 / 单个元素占用空间 = 数组长度。

```java
#include <iostream>
using namespace std;
int main() {

    int score[] = {1,2,3,4,5};

    int length = sizeof(score) / sizeof(score[0]); //计算数组长度

    cout<<length<<endl;
}

```

