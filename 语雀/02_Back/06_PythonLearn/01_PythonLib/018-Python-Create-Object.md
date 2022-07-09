# Python-Create-Object

# 1、Python Way

```python
class Book(object):

    # 1. __init__ 对象的初始化方法，创建对象的时候调用它。
    def __init__(self, book_id, book_name, book_price, book_publisher):

        # 2. __属性私有
        self.__book_publisher = book_publisher
        self.__book_price = book_price
        self.__book_name = book_name
        self.__book_id = book_id

    # 3. @property  相当于Getter方法，能访问到私有属性
    @property
    def book_publisher(self):
        return self.__book_publisher

    @property
    def book_price(self):
        return self.__book_price

    @property
    def book_name(self):
        return self.__book_name

    @property
    def book_id(self):
        return self.__book_id

    # Setter方法，能修改到私有属性
    @book_publisher.setter
    def book_publisher(self, book_publisher):
        self.__book_publisher = book_publisher

    @book_price.setter
    def book_price(self, book_price):
        self.__book_price = book_price

    @book_name.setter
    def book_name(self, book_name):
        self.__book_name = book_name

    @book_id.setter
    def book_id(self, book_id):
        self.__book_id = book_id


```

# 2、Java Way

```java
package com.wnx.model;



public class Book extends Object {

    // 1.Book()构造方法 对象的初始化方法，创建对象的时候调用它。
    public Book(String bookPublisher, Double bookPrice, String bookName, Integer bookId) {
        this.bookPublisher = bookPublisher;
        this.bookPrice = bookPrice;
        this.bookName = bookName;
        this.bookId = bookId;
    }

   // 2.private 属性私有
   private String bookPublisher;
   private Double bookPrice;
   private String bookName;
   private Integer bookId;
   
   //3。Getter方法，能访问到私有属性
    public String getBookPublisher() {
        return bookPublisher;
    }

    public Double getBookPrice() {
        return bookPrice;
    }

    public String getBookName() {
        return bookName;
    }

    public Integer getBookId() {
        return bookId;
    }

    //4.Setter方法，能修改到私有属性
    public void setBookPublisher(String bookPublisher) {
        this.bookPublisher = bookPublisher;
    }

    public void setBookPrice(Double bookPrice) {
        this.bookPrice = bookPrice;
    }

    public void setBookName(String bookName) {
        this.bookName = bookName;
    }

    public void setBookId(Integer bookId) {
        this.bookId = bookId;
    }
}


```

