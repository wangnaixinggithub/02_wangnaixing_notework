# Define-Pointer

- 声明指针需要 `*`关键字，`&`表示此指针指向哪一个变量的地址。 
- 指针存放的是变量的地址值

```c++
#include <iostream>
using namespace std;


int main() {
    int a = 10;
    //指针定义语法： 数据类型 * 变量名 = 指针指向变量a的地址
    int * p = &a;

    cout<<p; //0xca863ff664
}

```

```c++
#include <iostream>
using namespace std;


int main() {
     short num1 = 10;
     int num2 = 10;
     long  num3 = 10;
     long long num4 = 10;

     float  f1 = 0.2;
     double d1 = 0.2;

     char c1 = 'a';
     string str = "abc";

    short *p1 = &num1;
    int *p2 = &num2;
    long *p3 = &num3;
    long long *p4 = &num4;

    float *p5 = &f1;
    double *p6 = &d1;

    char *p7 = &c1;
    string *p8 = &str;
    
}

```

