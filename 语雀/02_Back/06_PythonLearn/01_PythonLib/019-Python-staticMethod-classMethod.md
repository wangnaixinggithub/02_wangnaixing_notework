# [Python-staticMethod-classMethod]()

# 1、Invoke Method Different

- 如何在类中定义 静态方法 和类方法

```python
class Book(object):

    @staticmethod
    def static_method():
        print('我是一个静态方法....')

    @classmethod
    def class_method(cls):
        print('我是一个类方法...')
```

- 类方法和静态方法调用区别

```python
from mudule import Book


def main():
    # 类方法 => 可以创建对象后，使用对象访问。
    book1 = Book()
    book1.class_method()

    # 静态方法 => 类访问 或者对象访问
    Book.static_method()
    book1.static_method()


if __name__ == '__main__':
    main()

```

![image-20220603104304175](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220603104304175.png)

# 2、In Java Is Same

```javascript
package com.wnx.model;

public class Book extends Object {

   public  static void staticMethod(){
       System.out.println("我是一个静态方法...");
   }

   public void classMethod(){
       System.out.println("我是一个类方法....");
   }


}
```

```javascript
package com.wnx.model;

public class Test {
    public static void main(String[] args) {
        //1.类方法 => 可以创建对象后，使用对象访问。
        Book book1 = new Book();
        book1.classMethod();


        //2.静态方法 => 类访问 或者对象访问
        Book.staticMethod();
        book1.staticMethod();

    }
}

```

# 3、Validation Test Result

```
我是一个类方法....
我是一个静态方法...
我是一个静态方法...
```

