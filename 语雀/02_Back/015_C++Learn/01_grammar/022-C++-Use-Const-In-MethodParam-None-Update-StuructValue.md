# Use-Const-In-MethodParam-None-Update-StuructValue

# 1、 Default Access Update

1、如果方法接受的是，结构体的指针。默认是可以通过指针修改结构体属性的。

```c++
#include <iostream>
using namespace std;


struct Student{
    long id;
    string name;
    int age;
};

/**
 * 修改结构体属性的值
 * @param student
 */
void updateStructValue(Student *student){
    student->id = 100;
    student->name="张三";
    student->age = 30;

}


int main() {



    struct Student student1;
    student1.id = 1;
    student1.name = "王乃醒";
    student1.age = 26;

    updateStructValue(&student1);

    cout<<student1.id<<endl;
    cout<<student1.name<<endl;
    cout<<student1.age<<endl;
    //100
    //张三
    //30
    
    return 0;


}
```

# 2、Use Const In Method Param

- 让指针变成指针常量。就不能通过指针修改结构体的值。

```java
/**
 * 修改结构体属性的值
 * @param student
 */
void updateStructValue( const Student *student){
    student->id = 100;
    student->name="张三";
    student->age = 30;

}
```

