# Class Contain Class

> 对于类中含有一个类，在执行构造过程中时候，一定是先初始化成员属性的构造方法，再走自己的构造方法。析构则相反。

```c++
#include <iostream>
using namespace std;

class Phone{
public:
    Phone(string name): name(name){
        cout<<"Phone Invoke...";
    }
    ~Phone(){
        cout<<"~Phone Invoke";
    }
private:
    string name;
};

class Student{
public:
    Student(string name,int age,string phoneName):name(name),age(age), phone(phoneName){
        cout<<"Student Invoke...";
    }
    ~Student(){
        cout<<"~Student Invoke";
    }


private:
    string name;
    int age;
    Phone phone;
};

int main() {
    Student student = Student("王乃醒",26,"18154622909");
//Phone Invoke...Student Invoke... ~Student Invoke~Phone Invoke

}

```

