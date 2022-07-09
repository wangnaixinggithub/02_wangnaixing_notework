# Controller方法的参数绑定

# 1、接收基本类型

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>添加图书页面</title>
</head>
<body>
<form th:action="@{/addBook}" method="post"  >

    图书名： <input type="text" name="name"> <br/>
    作者： <input type="text" name="author">  <br/>
    价格： <input type="text" name="price"> <br/>
    是否上架：<input type="radio" name="isPublic" value="true"> <input type="radio" name="isPublic" value="false"> <br/>
    <button type="submit">提交</button>
</form>
</body>
</html>
```

```java
   //接收基本类型
    @PostMapping("/addBook")
    public String addBook(String name,String author,Double price,Boolean isPublic){
        System.out.println(name);
        System.out.println(author);
        System.out.println(price);
        System.out.println(isPublic);
        return "addBook";
    }
```

# 2、接收POJO

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>添加图书页面2</title>
</head>
<body>
<form  th:action="@{/addBook2}" method="post"  >

    图书名： <input type="text" name="name"> <br/>
    作者： <input type="text" name="author">  <br/>
    价格： <input type="text" name="price"> <br/>
    是否上架：<input type="radio" name="publicStatus" value="true"> <input type="radio" name="publicStatus" value="false"> <br/>
    <button type="submit">提交</button>
</form>
</body>
</html>
```

```java
package com.wnx.springmvc.model;

import java.io.Serializable;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/5 18:50
 */
public class Book implements Serializable {
    private String name;
    private String author;
    private Double price;
    private Boolean publicStatus;

    @Override
    public String toString() {
        return "Book{" +
                "name='" + name + '\'' +
                ", author='" + author + '\'' +
                ", price=" + price +
                ", publicStatus=" + publicStatus +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public Boolean getPublicStatus() {
        return publicStatus;
    }

    public void setPublicStatus(Boolean publicStatus) {
        this.publicStatus = publicStatus;
    }
}

```

```java
    //接收POJO
    @PostMapping("/addBook2")
    public String addBook2(Book book){
        System.out.println(book);
        return "addBook2";
    }
```

# 3、接收POJO含POJO

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>添加图书页面</title>
</head>
<body>
<form th:action="@{/addBook3}" method="post"  >

    图书名： <input type="text" name="name"> <br/>
    作者姓名： <input type="text" name="author.name">  <br/>
    作者年龄： <input type="text" name="author.age">  <br/>
    价格： <input type="text" name="price"> <br/>
    是否上架：<input type="radio" name="publicStatus" value="true"> <input type="radio" name="publicStatus" value="false"> <br/>
    <button type="submit">提交</button>
</form>
</body>
</html>
```

```java
package com.wnx.springmvc.model;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/5 18:50
 */
public class Book2 {

    @Override
    public String toString() {
        return "Book2{" +
                "name='" + name + '\'' +
                ", author=" + author +
                ", price=" + price +
                ", publicStatus=" + publicStatus +
                '}';
    }

    private String name;
    private Author author;
    private Double price;
    private Boolean publicStatus;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Author getAuthor() {
        return author;
    }

    public void setAuthor(Author author) {
        this.author = author;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public Boolean getPublicStatus() {
        return publicStatus;
    }

    public void setPublicStatus(Boolean publicStatus) {
        this.publicStatus = publicStatus;
    }
}

```

```java
    //接收 POJO 含POJO
    @PostMapping("/addBook3")
    public String addBook3(Book2 book){
        System.out.println(book);
        return "addBook3";
    }
```

# 4、日期类型

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>添加图书页面</title>
</head>
<body>
<form th:action="@{/time}" method="post" >
    生日：<input type="date" name="birth">
    <button type="submit">提交</button>
</form>
</body>
</html>
```

日期类型SpringMvc并没有处理器帮我们处理，如果想直接获取，会出现如下异常打印。

```java
    @PostMapping("/time")
    public String time(Date birth){
        log.info("Date=>{}",birth);
        return "hello";
    }
```

```c++
[2022-05-08 15:04:08:046] [WARN] - org.springframework.web.servlet.handler.AbstractHandlerExceptionResolver.logException(AbstractHandlerExceptionResolver.java:208) - Resolved [org.springframework.web.method.annotation.MethodArgumentTypeMismatchException: Failed to convert value of type 'java.lang.String' to required type 'java.util.Date'; nested exception is org.springframework.core.convert.ConversionFailedException: Failed to convert from type [java.lang.String] to type [java.util.Date] for value '2022-02-11'; nested exception is java.lang.IllegalArgumentException]
```

- 可以用`@InitBinder`注解标识的解析处理日期的方法。

```java
    @InitBinder
    protected void initBinder(WebDataBinder binder) {
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
        binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true));
    }
	
    @PostMapping("/time")
    public String time(Date birth) {
        log.info("Date=>{}", birth);
        return "hello";
    }
```

- 或者可以用注解

```java
    //注解方式处理日期类型
    @PostMapping("/time")
    public String time(@DateTimeFormat(pattern="yyyy-MM-dd") Date birth){
        System.out.println(birth);
        return "time";
    }
```

# 6、数组接收

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form th:action="@{/checkbox}" method="post" >
    <h1>爱好</h1>
    代码：<input type="checkbox" name="hobby" value="代码">
    肥仔水：<input type="checkbox" name="hobby" value="肥仔水">
    篮球：<input type="checkbox" name="hobby" value="篮球">
    足球：<input type="checkbox" name="hobby" value="足球">
    <button type="submit">提交</button>
</form>
</body>
</html>
```

```java
    //input = checkbox  数组接收
    @PostMapping("/checkbox")
    public String time(String[] hobby){
        System.out.println(Arrays.toString(hobby));
        return "checkbox";
    }

```

# 7、List接收

> List必须包装在对象中！

```java


    //input = checkbox  List接收
    @PostMapping("/addClass")
    public String time(MyClass myClass){
        System.out.println(myClass);
        return "addClass";
    }

```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>添加图书页面</title>
</head>
<body>
<form  th:action="@{/addClass}" method="post"  >

    班级名： <input type="text" name="name"> <br/>
    学生1： <input type="text" name="students[0].name">  <br/>
    学生2： <input type="text" name="students[1].name">  <br/>
    学生3： <input type="text" name="students[2].name">  <br/>
    学生4： <input type="text" name="students[3].name">  <br/>
    学生5： <input type="text" name="students[4].name">  <br/>

    <button type="submit">提交</button>
</form>
</body>
</html>
```

