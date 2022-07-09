# Spring-AOP底层-JDK动态代理

- JDK的动态代理是基于接口实现的，UserDao接口，定义接口方法add(),此方法由UserDaoImpl实现，并重写了add()接口方法，实现了add()方法的业务逻辑。

```java
public interface UserDao {
    int add(int num1, int num2);
}
```

```java
public class UserDaoImpl implements UserDao{
    @Override
    public int add(int num1, int num2) {
        return num1 + num2;
    }
}
```

- 我们想增强UserDaoImpl的add()方法的业务逻辑，但是不改动原有的方法。增强方法的逻辑。这是便出现了动态代理技术。

- 需要创建代理对象的时候，注入被代理对象的引用。通过Proxy（代理）类的newProxyInstance()创建代理实例的方法，创建出代理对象。每次执行代理对象的方法时，会触发InvocationHandler执行处理器。由执行处理器通过反射执行目标方法。在执行目标方法的过程中，出现了几处我们能增强的地方就是通知了。
- 这个通知被分为前置通知，后置通知，结束通知，异常通知，以及环绕通知。

```java
public class UserDaoProxy {

    private  Object object;

    public UserDaoProxy(UserDao userDao) {
        this.object = userDao;
    }

    public  Object getUserDaoProxyInstance(){
       return Proxy.newProxyInstance(UserDaoProxy.class.getClassLoader(), new Class[]{UserDao.class}, (Object proxy, Method method, Object[] args) -> {
                 Object resultValue = null;
                     System.out.println("前置通知");
                 try {
                     System.out.println("环绕通知");
                     System.out.println("----------------在不改动UserDao实现的前提下，实现增强------------");
                      resultValue = method.invoke(object, args);
                     System.out.println("环绕通知");
                     System.out.println("后置通知");
                 }catch (Exception e){
                     System.out.println("异常通知");
                     System.out.println(e.getMessage());
                 }
             System.out.println("最终通知");

             return resultValue;

       });
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        UserDao userDao = (UserDao) new  UserDaoProxy(new UserDaoImpl()).getUserDaoProxyInstance();
        System.out.println(userDao.add(1,2));
        System.out.println(userDao);//任何方法都会切入，包括userDao.toString()方阿飞
    }
}
```

```c#
前置通知
环绕通知
----------------在不改动UserDao实现的前提下，实现增强------------
环绕通知
后置通知
最终通知
3
前置通知
环绕通知
----------------在不改动UserDao实现的前提下，实现增强------------
环绕通知
后置通知
最终通知
```

