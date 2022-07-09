# C++-Define-Class

- 在C++定义类，和Java不同的是，他多了析构函数。还有拷贝构造方法

```c++
#include <iostream>
using namespace std;

class Person{
    //1.私有属性
private:
    long id;
    string name;
    int  age;

public:
    //2.无参构造方法
    Person(){}

    //3.全参构造方法
    Person(long id,string name,int age){
        this->id = id;
        this->name = name;
        this->age =age;
    }

    //4.拷贝构造方法
    Person(const Person &person){
        this->id = person.id;
        this->age = person.age;
        this->name = person.name;
    }

    //5.Setter Getter 方法
    void setId(int id){
        this->id = id;
    }
    int getId() const{
        return this->id;
    }
    void setName(string name){
        this->name = name;
    }
    string getName(){
        return this->name;
    }
    void setAge(int age){
        this->age = age;
    }
    int getAge(){
        return this->age;
    }
    //6.无参析构函数
    ~Person()
    {

    }


};



int main() {

    Person p =  Person(1,"王乃醒",26);
    cout<<p.getId();
    cout<<p.getName();
    cout<<p.getAge();


    Person p2 = Person();
    p2.setId(2);
    p2.setName("黄洁莹");
    p2.setAge(27);



}

```

- 参构造函数的简化写法-初始化列表

```java
    Person(long id,string name,int age):id(id),name(name),age(age){}
```

