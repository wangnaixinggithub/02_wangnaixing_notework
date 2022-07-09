# Java对象-Object类

# 1、所有对象的最大父类

```java
public class Object {
      //创建并返回此对象的副本
	  protected native Object clone() throws CloneNotSupportedException;
    
    //指示其他对象是否“等于”这个对象。
	  public boolean equals(Object obj) {}
     
    //返回对象的哈希码值
	  public native int hashCode();
     
    //唤醒正在等待该对象监视器的单个线程。
	  public final native void notify();
    
    //唤醒所有在该对象监视器上等待的线程。
	  public final native void notifyAll();
  
    //返回对象的字符串表示形式
	  public String toString() {}  
   
    // 让当前线程等待状态，直到另一个线程调用该对象的notify()方法或notifyAll()方法
      public final native void wait(long timeout) throws InterruptedException;
    
      //当垃圾回收器决定回收某对象时，就会调用该对象的finalize()方法
      protected void finalize() throws Throwable { }
}
```

