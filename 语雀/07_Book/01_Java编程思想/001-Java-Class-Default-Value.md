# Java-Class-Default-Value

- 在类中的变量，即便没有被初始化，Java会提供给他们默认值。

```java
package com.wnx.model;

public class Student {
    private int number1;

    private long number2;

    private short number3;

    private float number4;

    private double number5;


    private char char1;


    public static void main(String[] args) {
        Student student = new Student();
        System.out.println(student.number1);
        System.out.println(student.number2);
        System.out.println(student.number3);
        System.out.println(student.number4);
        System.out.println(student.number5);
        System.out.println(student.char1);

        //0
        //0
        //0
        //0.0
        //0.0
        // 空格
    }
}




```

