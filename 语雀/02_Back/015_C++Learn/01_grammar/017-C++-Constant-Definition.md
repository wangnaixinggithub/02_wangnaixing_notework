# Constant Definition

> 在C++中，如何声明常量？常量是一个只读量，不能进行更改。

# 1、short、 int、 long、 long long 

```c++
#include <iostream>
using namespace std;

int main() {
    const short num1 = 10;
    const int num2 = 10;
    const long  num3 = 10;
    const long long num4 = 10;

    num1 = 20;
    num2 = 20;
    num3 = 20;
    num4 = 20;

}
```

![image-20220615083902715](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220615083902715.png)

# 2、double 、float

```c++
#include <iostream>
using namespace std;


int main() {
  const float  f1 = 0.2;
  const double d1 = 0.2;
  f1 = 0.3;
  d1 = 0.3;

}

```

![image-20220615083956972](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220615083956972.png)

# 3、char、 string

```c++
#include <iostream>
using namespace std;


int main() {
   const char c1 = 'a';
   const string str = "abc";

   c1 = 'b';
   str = "def";
   

}
```

![image-20220615084209094](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220615084209094.png)

# 4、Point

- 1、常量指针

```c++
#include <iostream>
using namespace std;


int main() {
    int a = 10;
    int b = 20;
    int c = 30;
    
    const int *p1 = &a;
    //Error ,指针指向的值不可以更改。
    *p1 = 300;

}
```

```shell
C:/Users/Administrator.DESKTOP-E0KTJ20/CLionProjects/untitled/main.cpp:12:9: error: assignment of read-only location '* p1'
   12 |     *p1 = 300;
      |     ~~~~^~~~~
ninja: build stopped: subcommand failed.
```

- 2、指针常量

```c++
#include <iostream>
using namespace std;


int main() {
    int a = 10;
    int b = 20;
    int c = 30;

     int * const p1 = &a;
    //Error ,指针指向不可以更改。
    p1 = &b;
    p1 = &c;

}
```

```shell
C:/Users/Administrator.DESKTOP-E0KTJ20/CLionProjects/untitled/main.cpp:12:8: error: assignment of read-only variable 'p1'
   12 |     p1 = &b;
      |     ~~~^~~~
C:/Users/Administrator.DESKTOP-E0KTJ20/CLionProjects/untitled/main.cpp:13:8: error: assignment of read-only variable 'p1'
   13 |     p1 = &c;
      |     ~~~^~~~
```

