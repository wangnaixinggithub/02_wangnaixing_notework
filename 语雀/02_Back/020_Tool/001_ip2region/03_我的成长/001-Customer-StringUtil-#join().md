# Customer-StringUtil-#join()

> 手写一个StringUtils的join()方法，功能在于将Collection 转换为字符串

# 1、extends `org.springframework.util.StringUtils`

```java
public class StringUtils extends org.springframework.util.StringUtils {
	
}
```

# 2、Create  #Join()

```java
public class StringUtils extends org.springframework.util.StringUtils {
    
	public static String join(Collection<?> coll, String delim) {
        return collectionToDelimitedString(coll, delim);
    }
}
```

# 3、Create #collectionToDelimitedString()



```java
public class StringUtils extends org.springframework.util.StringUtils {

    public static String collectionToDelimitedString(@Nullable Collection<?> coll, String delim, String prefix, String suffix) {
            if (CollectionUtils.isEmpty(coll)) {
                return "";
            } else {

                //1、计算String 总长度
                int totalLength = coll.size() * (prefix.length() + suffix.length()) + (coll.size() - 1) * delim.length();


                //2、
                Object element;
                for(Iterator var5 = coll.iterator(); var5.hasNext(); totalLength += String.valueOf(element).length()) {
                    element = var5.next();
                }

                StringBuilder sb = new StringBuilder(totalLength);

                //3、迭代器做循环 SpringBuilder 进行组接
                Iterator<?> it = coll.iterator();
                while(it.hasNext()) {
                    sb.append(prefix).append(it.next()).append(suffix);
                    if (it.hasNext()) {
                        sb.append(delim);
                    }
                }

                //4、返回
                return sb.toString();
            }
        }
}
```



# 4、Overloading #collectionToDelimitedString()

> 重载：提升方法的通用性

```java
package com.wnx.utils;

import org.springframework.lang.Nullable;
import org.springframework.util.CollectionUtils;

import java.util.Collection;
import java.util.Iterator;

/**
 * 对外提供将集合转为String字符串
 */
public class StringUtils extends org.springframework.util.StringUtils {

    public static String join(Collection<?> coll, String delim) {
        return collectionToDelimitedString(coll, delim);
    }
    
    public static String collectionToDelimitedString(@Nullable Collection<?> coll, String delim, String prefix, String suffix) {
        if (CollectionUtils.isEmpty(coll)) {
            return "";
        } else {

            //1、计算String 总长度
            int totalLength = coll.size() * (prefix.length() + suffix.length()) + (coll.size() - 1) * delim.length();


            //2、
            Object element;
            for(Iterator var5 = coll.iterator(); var5.hasNext(); totalLength += String.valueOf(element).length()) {
                element = var5.next();
            }

            StringBuilder sb = new StringBuilder(totalLength);

            //3、迭代器做循环 SpringBuilder 进行组接
            Iterator<?> it = coll.iterator();
            while(it.hasNext()) {
                sb.append(prefix).append(it.next()).append(suffix);
                if (it.hasNext()) {
                    sb.append(delim);
                }
            }

            //4、返回
            return sb.toString();
        }
    }

    public static String collectionToDelimitedString(@Nullable Collection<?> coll, String delim) {
        return collectionToDelimitedString(coll, delim, "", "");
    }

    public static String collectionToCommaDelimitedString(@Nullable Collection<?> coll) {
        return collectionToDelimitedString(coll, ",");
    }
}

```

# 5、Validation Test

```java
package com.wnx;

import com.wnx.model.People;
import com.wnx.utils.StringUtils;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.ArrayList;
import java.util.List;

@SpringBootTest
@Slf4j
class BackApplicationTests {

    @Test
    public void test() {
        List<String> list = new ArrayList<>();
        list.add("中国");
        list.add("广西");
        list.add("南宁");
        list.add("西乡塘区");

        Assertions.assertEquals("中国广西南宁西乡塘区",StringUtils.join(list, ""));
    }

}

```

