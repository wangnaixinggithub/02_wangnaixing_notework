# Strategy Pattern And Factory pattern Remove-if-else

# 1、Before code Optimization

```java
package com.wnx;

public class Test1 {

    public static void main(String[] param) {
        String paramStr = "";
        testMethod(paramStr);
    }

    private  static void testMethod(String paramStr) {
        if (paramStr.equals("A")){
            A a = new A();
            a.aPrintln(paramStr);
        } else if (paramStr.equals("B")) {

            B b = new B();
            b.aPrintln(paramStr);
        }else {
            C c = new C();
            c.aPrintln(paramStr);
        }

    }
}



class A implements printlnString{
    public void aPrintln(String a) {
        System.out.println(a);
    }
}

class B implements printlnString{
    public void aPrintln(String a) {
        System.out.println(a);
    }
}


class C implements printlnString{
    public void aPrintln(String a) {
        System.out.println(a);
    }
}
interface printlnString{
    public void aPrintln(String a);
}


```

# 2、After code Optimization

```java
package com.wnx.model;

public class A implements PrintlnString{
    @Override
    public void aPrintln(String a) {
        System.out.println("----------A----------");
        System.out.println(a);
    }
}

package com.wnx.model;

public class B implements PrintlnString{
    @Override
    public void aPrintln(String a) {
        System.out.println("----------B----------");
        System.out.println(a);
    }
}

package com.wnx.model;

public class C implements PrintlnString{

    @Override
    public void aPrintln(String a) {
        System.out.println("----------C----------");
        System.out.println(a);
    }
}


package com.wnx.model;

public interface PrintlnString {
     void aPrintln(String a);
}

```

```java
package com.wnx.factory;

import com.wnx.model.A;
import com.wnx.model.B;
import com.wnx.model.C;
import com.wnx.model.PrintlnString;
import java.util.HashMap;
import java.util.Map;

public class PrintServiceFactory {
    private static final Map<String, PrintlnString> map = new HashMap<>(3);

    static {
        map.put("A",new A());
        map.put("B",new B());
        map.put("C",new C());
    }

    public static PrintlnString getPrintService(String paramStr){
        return map.get(paramStr);
    }

}
```

```java
public class Test1 {

    public static void main(String[] param) {
        String paramStr = "C";
        testMethod(paramStr);
    }

    private  static void testMethod(String paramStr) {
        PrintlnString printlnString = PrintServiceFactory.getPrintService(paramStr);
        String inputMsg = "王乃醒是一个大帅哥！！！";
        printlnString.aPrintln(inputMsg);

    }
}
```

