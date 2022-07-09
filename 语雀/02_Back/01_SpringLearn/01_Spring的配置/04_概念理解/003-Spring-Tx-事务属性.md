# Spring-Tx-属性含义

```properties
事务的传播行为=propagation

事务的隔离级别=isolation

事务的超时时间=timeout

事务是否可读=readOnly

设置出现哪些异常事务回滚=rollbackFor

设置事务出现了哪些异常事务不回滚= noRollbackFor
```

## 1、propagation

```java
package org.springframework.transaction.annotation;
public enum Propagation {
    REQUIRED(0), //如果一个事务方法里面有调用了另一个事务方法，那么事务就使用当前方法的。
    SUPPORTS(1), 
    MANDATORY(2), 
    REQUIRES_NEW(3),//如果一个事务方法里面有调用了另一个事务方法，那么事务就使用另一个方法的。
    NOT_SUPPORTED(4), 
    NEVER(5), // 不使用事务
    NESTED(6); //如果一个事务方法里面有调用了另一个事务方法，那么事务就嵌套执行。
    private final int value;
    private Propagation(int value) {
        this.value = value;
    }
    public int value() {
        return this.value;
    }
}
```

> `propagation = Propagation.REQUIRED`

```java
    @Transactional(propagation = Propagation.REQUIRED,rollbackFor = Exception.class)
    public void add(){
        update();

    }
    public void  update(){

    }
//如果add()有事务，调用update(),update()使用add()方法的事务。
```

```java
   public void add(){
        update();

    }
    @Transactional(propagation = Propagation.REQUIRED,rollbackFor = Exception.class)
    public void  update(){
		
    }
//如果add()方法没有事务，调用update()方法之后，创建一个新的事务。
```

> `propagation = Propagation.REQUIRES_NEW`

```java
    @Transactional(propagation = Propagation.REQUIRES_NEW,rollbackFor = Exception.class)
    public void add(){
        update();

    }
    public void  update(){

    }
}
//不管add()方法有没有事务，在调用update()方法的时候，都会创建一个新的事务。
```

## 2、isolation

```java
package org.springframework.transaction.annotation;
public enum Isolation {
    DEFAULT(-1), //默认
    READ_UNCOMMITTED(1), //读未提交
    READ_COMMITTED(2), //读已提交
    REPEATABLE_READ(4), //可重复读
    SERIALIZABLE(8); //串行化
    private final int value;
    private Isolation(int value) {
        this.value = value;
    }
    public int value() {
        return this.value;
    }
}

```

脏读，不可重复读，幻读。

## 3、timeout

```properties
默认值为-1，事务必须在超时时间内提交，如果不提交就回滚。
以秒为单位
```

## 4、readOnly

```properties
默认为false,表示可以进行读：查询操作，写：增删改操作
true，表示只能进行读操作。
```

## 5、rollbackFor

```
出现何种异常，事务会回滚。
```

## 6、 noRollbackFor

```
出现何种异常，事务不回滚。
```

