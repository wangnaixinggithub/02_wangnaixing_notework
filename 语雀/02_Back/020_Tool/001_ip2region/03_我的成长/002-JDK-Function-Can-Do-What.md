# JDK-Function-Can-Do-What

> 这个代码告诉我JDK的Funcation 接口的用途。

# 1、Provide Model

```java
package com.wnx.utils;

public interface StringPool {
    String EMPTY = "";
}

```

```java
package com.wnx.model;

import com.wnx.utils.StringPool;
import com.wnx.utils.StringUtils;
import lombok.*;

import java.util.LinkedHashSet;
import java.util.Set;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class People {
    
    private String country;
    private String  province;
    private String city;
    private String region;


    public String getAddress(){
        Set<String> set = new LinkedHashSet<>();
        set.add(country);
        set.add(province);
        set.add(city);
        set.add(region);
       return StringUtils.join(set, StringPool.EMPTY);
    }
}

```

# 2、Create Utils Use  Function Interface



```java
package com.wnx.utils;

import com.wnx.model.People;
import org.springframework.lang.Nullable;

import java.util.function.Function;

public class PeopleUtils {
    @Nullable
    public static String readPeople(@Nullable People people, Function<People, String> function) {
        if (people == null) {
            return null;
        }
        return function.apply(people);
    }
}

```

# 3、Use Util Can Invoke Model Method

```javascript
@SpringBootTest
@Slf4j
class BackApplicationTests {

    @Test
    public void test() {
        People people = new People("中国","广西","南宁","西乡塘区");

        String country = PeopleUtils.readPeople(people, People::getCountry);
        String province = PeopleUtils.readPeople(people, People::getProvince);
        String city = PeopleUtils.readPeople(people, People::getCity);
        String regin = PeopleUtils.readPeople(people, People::getRegion);
        String address = PeopleUtils.readPeople(people, People::getAddress);


        Assertions.assertEquals("中国",country);
        Assertions.assertEquals("广西",province);
        Assertions.assertEquals("南宁",city);
        Assertions.assertEquals("西乡塘区",regin);
        Assertions.assertEquals("中国广西南宁西乡塘区",address);

        
    }

}
```

