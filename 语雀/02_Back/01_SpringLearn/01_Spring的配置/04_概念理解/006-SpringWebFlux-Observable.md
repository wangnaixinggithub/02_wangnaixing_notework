# Spring-响应式编程-Observable

- 继承Observable接口，具备观察的能力

```java
public class ObserverDemo extends Observable {｝
```

- 通过使用继承Observable类的方法addObserver()添加观察者。

```java
 ObserverDemo observerDemo = new ObserverDemo();
        observerDemo.addObserver((o,arg)->{
            System.out.println("观察者1->发生变化了。。哈哈哈");
        });
        observerDemo.addObserver((o,arg)->{
            System.out.println("观察者2->发生变化了。。哈哈哈");
        });
```

- 通过使用继承Observable类的方法setChanged()方法，设置监听变化。

```java
observerDemo.setChanged();
```

- 通过使用继承Observable类的方法notifyObservers()方法，获取到变化通知。

```
observerDemo.notifyObservers();
```

- 查看控制台输出，成功获取到观察者监听的内容

```java
public class ObserverDemo extends Observable {
    public static void main(String[] args) {
        //1.注册观察者
        ObserverDemo observerDemo = new ObserverDemo();
        observerDemo.addObserver((o,arg)->{
            System.out.println("观察者1->发生变化了。。哈哈哈");
        });
        observerDemo.addObserver((o,arg)->{
            System.out.println("观察者2->发生变化了。。哈哈哈");
        });
        //2.设置变化
        observerDemo.setChanged();
        //3.获取变化通知
        observerDemo.notifyObservers();
    }

```

```coffeescript
观察者2->发生变化了。。哈哈哈
观察者1->发生变化了。。哈哈哈
```

