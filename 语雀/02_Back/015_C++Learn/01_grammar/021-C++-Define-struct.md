# Define-struct

# 1、Denfine struct

- 定义C++中结构体变量。相等于自定义的数据类型。

```c++
#include <iostream>
using namespace std;

int main() {
    //定义结构体
    struct Student{
        long id;
        string name;
        int age;
    };
    //使用结构体
    struct Student student;
    
    student.id = 1;
    student.name = "王乃醒";
    student.age = 26;

    cout<<student.id<<student.name<<student.age;

    return 0;


}
```

- 结构体，嵌套。 

```c++
#include <iostream>
using namespace std;

int main() {

    struct Phone{
        long id;
        string phoneName;
        double price;
    };


    struct Student{
        long id;
        string name;
        int age;
        struct Phone phone;
    };

    struct Student student1;
    student1.id = 1;
    student1.name = "王乃醒";
    student1.age = 26;
    student1.phone.id = 1;
    student1.phone.phoneName= "小米";
    student1.phone.price = 2998.4;

    struct Student student2;
    student2.id = 2;
    student2.name = "黄洁莹";
    student2.age = 25;
    student2.phone.id = 2;
    student2.phone.phoneName= "苹果";
    student2.phone.price = 3998.4;


    struct Student student3;
    student3.id = 3;
    student3.name = "杨川庆";
    student3.age = 25;
    student3.phone.id = 3;
    student3.phone.phoneName= "华为";
    student3.phone.price = 4998.4;

    //定义结构体数组
    struct Student studentArray[3] = {
            student1,
            student2,
            student3
    };


    return 0;


}
```



# 2、Define struct Array

```c++
#include <iostream>
using namespace std;

int main() {

    struct Student{
        long id;
        string name;
        int age;
    };

    struct Student student1;
    student1.id = 1;
    student1.name = "王乃醒";
    student1.age = 26;

    struct Student student2;
    student2.id = 2;
    student2.name = "黄洁莹";
    student2.age = 25;

    struct Student student3;
    student3.id = 3;
    student3.name = "杨川庆";
    student3.age = 25;

    //定义结构体数组
    struct Student studentArray[3] = {
            student1,
            student2,
            student3
    };


    return 0;


}
```

