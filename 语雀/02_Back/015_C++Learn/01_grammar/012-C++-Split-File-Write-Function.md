# Split-File-Write-Function

> 一个.cpp文件太多函数时，我们可以通过另一个.cpp文件进行函数管理，然后去引入，进而调用方法，减少代码冗余。

# 1、Create 

- 创建.h的头文件，创建.cpp的源文件，名字取模块名。

![image-20220614171948278](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220614171948278.png)

# 2、Writer Function Define

```c++
//swap.h

//1.所有需要的Jar放在这里
#include <iostream>
using namespace std;

//2.声明函数
void swap(int num1, int num2);
```

# 3、Writer Function Implement

```c++
//swap.cpp

//1.引入头文件
#include "swap.h"

//2.实现函数
void swap(int num1, int num2){
    cout << "交换前：" << endl;
    cout << "num1 = " << num1 << endl;
    cout << "num2 = " << num2 << endl;

    int temp = num1;
    num1 = num2;
    num2 = temp;

    cout << "交换后：" << endl;
    cout << "num1 = " << num1 << endl;
    cout << "num2 = " << num2 << endl;

}
```

# 4、Use It

```c++
//引入头文件既可以使用了
#include "swap.h"


int main() {
    int a = 2;
    int b = 3;
    swap(a,b);

    cout<< "打印下初始的[a,b]  " << a <<","<< b<< endl;


}
```

