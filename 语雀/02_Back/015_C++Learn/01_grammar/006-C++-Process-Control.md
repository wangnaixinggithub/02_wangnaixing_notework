# C++-Process-Control

# 1、if

```java
#include <iostream>
using namespace std;
int main() {
    int score = 41;

    if (score > 40){
        cout<<"Invoke Do Some Thing"<<endl;
    }
    
}

```

# 2、if else

```c++
#include <iostream>
using namespace std;
int main() {
    int score = 40;

    if (score > 40){
        cout<<"Invoke Do Some Thing"<<endl;
    } else{
        cout<<"Else,Invoke Do Some Thing"<<endl;
    }

}

```

# 3、if elseif..else

```c++
#include <iostream>
using namespace std;
int main() {
    int score = 10;

    if (score > 40){
        cout<<1<<endl;
    } else if ( score >= 30 && score <=40 ){
        cout<<2<<endl;
    }else if (score >=20 && score <= 30){
        cout<<3<<endl;
    } else{
        cout<<4<<endl;
    }

}

```

# 4、三目

**语法：**`表达式1 ? 表达式2 ：表达式3`

**解释：**

如果表达式1的值为真，执行表达式2，并返回表达式2的结果；

如果表达式1的值为假，执行表达式3，并返回表达式3的结果。

```c++
#include <iostream>
using namespace std;
int main() {
    int a = 10;
    int b = 20;
    int c = 0;

    c = a > b ? a : b;
    
    cout << "c = " << c << endl;

}
```

# 5、switch case default

```c++
#include <iostream>
using namespace std;
int main() {
    int  condition = 3;
    switch (condition) {
        case 0:
            cout<<0<<endl;
            break;
        case 1:
            cout<<1<<endl;
            break;
        case 2:
            cout<<2<<endl;
            break;
        default:
            cout<<"default"<<endl;
            break;
    }

}

```

# 6、while And do..while

```c++
#include <iostream>
using namespace std;
int main() {
  int num = 0;
    while (num <= 10){
        cout << "num = " << num << endl;
        num++;
    }
    
    return 0;

}

//num = 0
//num = 1
//num = 2
//num = 3
//num = 4
//num = 5
//num = 6
//num = 7
//num = 8
//num = 9
//num = 10


```

```c++
#include <iostream>
using namespace std;
int main() {
  int num = 0;
   do{
       cout << "num = " << num << endl;
       num++;
   } while (num <= 10);
}

//num = 0
//num = 1
//num = 2
//num = 3
//num = 4
//num = 5
//num = 6
//num = 7
//num = 8
//num = 9
//num = 10
```

# 7、for

```c++
#include <iostream>
using namespace std;
int main() {
    int numberArr[5] = {1,2,3,4,5,};
    //1.支持下标遍历
    for (int i = 0; i < 5; ++i) {
        cout<<numberArr[i]<<endl;
    }

    //1
    //2
    //3
    //4
    //5

    //2.支持数组遍历
    for (int item:numberArr) {
        cout<<item<<endl;
    }
    
    //1
    //2
    //3
    //4
    //5

}
```

# 8、break

- 跳出当前循环

```c++
#include <iostream>
using namespace std;
int main() {
    for (int i = 0; i < 10; i++)
    {
        if (i == 5)
        {
            break; //跳出整个循环
        }
        cout << i << endl;
    }
    return 0;
}
//0
//1
//2
//3
//4

```

- 一个break只能作用在他所在的循环。

```java
#include <iostream>
using namespace std;
int main() {

    for (int i = 0; i < 10; i++){

        for (int j = 0; j < 10; j++){
            if (j == 5){
                break;
            }
            cout << "*" << " ";
        }

        cout << endl;
    }
}

//* * * * * 打印10次 - 外部for
//* * * * * 打印5个花瓣 - 内部for，受到break影响
//* * * * *
//* * * * *
//* * * * *
//* * * * *
//* * * * *
//* * * * *
//* * * * *
//* * * * *

```

# 9、continue

- 跳过本次循环中余下尚未执行的语句，继续执行下一次循环

```java
#include <iostream>
using namespace std;
int main() {

    for (int i = 0; i < 10; i++){
        if (i % 2 == 0){ //如果是偶次,就不执行这次循环内容，继续往下循环。
            continue;
        }

        cout << i << endl;
    }




    return 0;
}
//1
//3
//5
//7
//9
```

# 10、goto 

- 打破顺序执行，进行跳转执行。

```java
#include <iostream>
using namespace std;
int main() {



    cout << 1 << endl;
    cout << 2 << endl;

    goto FLAG1;
    cout << 3 << endl;
    cout << 4 << endl;

    FLAG1: //跳到这里开始执行
    cout << 5 << endl;

}
//1
//2
//5
```

