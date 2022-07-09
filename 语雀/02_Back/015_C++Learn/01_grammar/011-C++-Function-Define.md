# Function-Define

# 1、Two Way Define Function



- 定义函数并在main()方法中调用执行。

```java
#include <iostream>
    
int sum(int num1,int num2){
    return num1 + num2;
}

using namespace std;
int main() {
   int result =  sum(2,3);
    cout<<result<<endl;
}

```

- 先声明，后面再写实现。

```java
#include <iostream>
int sum(int num1,int num2);
using namespace std;

int main() {
   int result =  sum(2,3);
    cout<<result<<endl;
}

int sum(int num1,int num2){
    return num1 + num2;
}

```

# 2、Pass By Value

- 函数方法的形参和实参相互独立的，不会互相影响。

```c++
#include <iostream>
using namespace std;


int main() {
    int a = 2;
    int b = 3;
    swap(a,b);

    cout<< "打印下初始的[a,b]  " << a <<","<< b<< endl;


}

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

