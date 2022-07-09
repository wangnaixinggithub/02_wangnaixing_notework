# JDK8-Stream

# 1、constructorMethod

```java
// permissionList指所有权限列表
// 为集合创建串行流对象
Stream<UmsPermission> stream = permissionList.stream();

// 为集合创建并行流对象
tream<UmsPermission> parallelStream = permissionList.parallelStream();
```

# 2、filter()

- 对Stream中的元素进行过滤操作，当设置条件返回true时返回相应元素

```java
// 获取权限类型为目录的权限
List<UmsPermission> dirList = permissionList.stream()
    .filter(permission -> permission.getType() == 0)
    .collect(Collectors.toList());
```

# 3、map()

- 对Stream中的元素进行转换处理后获取。

```java
// 获取所有权限的id组成的集合
List<Long> idList = permissionList.stream()
    .map(permission -> permission.getId())
    .collect(Collectors.toList());
```

# 4、limit()

```java
// 获取前5个权限对象组成的集合
List<UmsPermission> firstFiveList = permissionList.stream()
    .limit(5)
    .collect(Collectors.toList());
```

# 5、count（）

```java
// count操作：获取所有目录权限的个数
long dirPermissionCount = permissionList.stream()
    .filter(permission -> permission.getType() == 0)
    .count();
```

# 6、sorted()

- 将所有权限按先目录后菜单再按钮的顺序排序 目录是3，菜单是2 ，目录是1

```java
List<UmsPermission> sortedList = permissionList.stream()
    .sorted((permission1,permission2)->{return permission1.getType().compareTo(permission2.getType());})
    .collect(Collectors.toList());
```

# 7、skip()

```java
// 跳过前5个元素，返回后面的
List<UmsPermission> skipList = permissionList.stream()
    .skip(5)
    .collect(Collectors.toList());
```

# 8、toMap()

- 把集合转为Map

```java
// 将权限列表以id为key，以权限对象为值转换成map
Map<Long, UmsPermission> permissionMap = permissionList.stream()
    .collect(Collectors.toMap(permission -> permission.getId(), permission -> permission));
```

# 9、toList()

```java
        List<Integer> list = Arrays.asList(1, 6, 3, 4, 6, 7, 9, 6, 20);
        List<Integer> listNew = list.stream()
                .filter(x -> x % 2 == 0)
                .collect(Collectors.toList());
```

# 10、toSet()

```java
     Set<Integer> set = list.stream()
                .filter(x -> x % 2 == 0)
                .collect(Collectors.toSet());
```

# 11、max()

- 获取集合中最长到字符串元素

```java
package com.wnx.Stream.聚合;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.Optional;

public class Test {
    public static void main(String[] args) {
        
        List<String> list = Arrays.asList("1", "12", "123", "1234", "12345");
        
        Optional<String> max = list.stream()
                .max(Comparator.comparing(String::length));
        
        System.out.println("最长的字符串：" + max.get());

    }
}

```

![](008-JDK8-Stream.assets/QQ%E6%88%AA%E5%9B%BE20211111085746.png)

- 获取集合中最大的整数

```java
public class Test2 {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(7, 6, 9, 4, 11, 6);

        // way 1
        Optional<Integer> max = list.stream()
                .max(Integer::compareTo);

 		 // way 2
        Optional<Integer> max2  = list.stream()
                .max(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1.compareTo(o2);
            }
        });
        
       // way 3
        Optional<Integer> max3 = list.stream().
                max(Comparator.comparingInt(Integer::intValue));


        System.out.println("自然排序的最大值：" + max.get());

        System.out.println("自定义排序的最大值：" + max2.get());

        System.out.println("自然排序的最大值：" + max3.get());

    }
}

```

![](008-JDK8-Stream.assets/QQ%E6%88%AA%E5%9B%BE20211111090528.png)

- 获取POJO 的整数值的最大值

```java
@Data
public class Person {
    private String name;  // 姓名
    private int salary; // 薪资
    private int age; // 年龄
    private String sex; //性别
    private String area;  // 地区
}
```

```java
public class Test3 {
    public static void main(String[] args) {
    
        List<Person> personList = new ArrayList<Person>();
        personList.add(new Person("Tom", 8900, 23, "male", "New York"));
        personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
        personList.add(new Person("Anni", 8200, 24, "female", "New York"));
        personList.add(new Person("Owen", 9500, 25, "male", "New York"));
        personList.add(new Person("Alisa", 7900, 26, "female", "New York"));

        Optional<Person> max = personList.stream()
                .max(Comparator.comparingInt(Person::getSalary));


        System.out.println("员工工资最大值：" + max.get().getSalary());

    }
}
```

# 12、count()

- 获取个数

```java
package com.wnx.Stream.聚合;

import java.util.Arrays;
import java.util.List;

public class Test4 {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(7, 6, 4, 8, 2, 11, 9);

        long count = list.stream()
                .filter(x -> x > 6)
                .count();

        System.out.println("list中大于6的元素个数：" + count);
    }
}

```

# 13、reduce()

- 进行求和约归操作

```java

public class Test {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 3, 2, 8, 11, 4);

        // 求和方式1
        Optional<Integer> sum  = list.stream().reduce((x, y) -> x + y);
        
        // 求和方式2
        Optional<Integer> sum2 = list.stream().reduce(Integer::sum);
        
        // 求和方式3
        Integer sum3 = list.stream().reduce(0, Integer::sum);

        System.out.println("list求和：" + sum.get() + "," + sum2.get() + "," + sum3);

        // 求乘积
        Optional<Integer> product  = list.stream().reduce((x, y) -> x * y);
        System.out.println("list求积：" + product.get());


        // 求最大值方式1
        Optional<Integer> max = list.stream().reduce((x, y) -> x > y ? x : y);
        Optional<Integer> max2 = list.stream().reduce(Integer::max);
      
        System.out.println("list求和：" + max.get() + "," + max2.get());

    }
}

```

- 求POJO属性和

```java
public class Test2 {
    public static void main(String[] args) {
        List<Person> personList = new ArrayList<Person>();
        personList.add(new Person("Tom", 8900, 23, "male", "New York"));
        personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
        personList.add(new Person("Anni", 8200, 24, "female", "New York"));
        personList.add(new Person("Owen", 9500, 25, "male", "New York"));
        personList.add(new Person("Alisa", 7900, 26, "female", "New York"));

        // 求工资之和方式1：
        Optional<Integer> sumSalary  = personList.stream()
                .map(Person::getSalary)
                .reduce(Integer::sum);

        // 求工资之和方式2：
        Integer sumSalary2 = personList.stream()
                .reduce(0, (sum, p) -> sum += p.getSalary(),
                (sum1, sum2) -> sum1 + sum2);


        // 求工资之和方式3：
        Integer sumSalary3 = personList.stream()
                .reduce(0, (sum, p) -> sum += p.getSalary(), Integer::sum);

        System.out.println("工资之和：" + sumSalary.get() + "," + sumSalary2 + "," + sumSalary3);

        // 求最高工资方式1：
        Integer maxSalary = personList.stream()
                .reduce(0, (max, p) -> max > p.getSalary() ? max : p.getSalary(),
                Integer::max);

        // 求最高工资方式2：
        Integer maxSalary2 = personList.stream()
                .reduce(0, (max, p) -> max > p.getSalary() ? max : p.getSalary(),
                (max1, max2) -> max1 > max2 ? max1 : max2);


        System.out.println("最高工资：" + maxSalary + "," + maxSalary2);
    }
}

```

![](008-JDK8-Stream.assets/QQ%E6%88%AA%E5%9B%BE20211111095314.png)

# 14、forEach()

- 遍历

```java
package com.wnx.Stream.遍历匹配;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class Test {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(7, 6, 9, 3, 8, 2, 1);
        
        // 遍历输出符合条件的元素
        list.stream().filter(x -> x > 6).forEach(System.out::println);

    }
}

```



# 14、findFirst()

- 匹配第一个

```java
package com.wnx.Stream.遍历匹配;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class Test {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(7, 6, 9, 3, 8, 2, 1);
        

        // 匹配第一个
        Optional<Integer> findFirst = list.stream().filter(x -> x > 6).findFirst();


        System.out.println("匹配第一个值：" + findFirst.get());




    }
}

```

# 15、findAny()

- 匹配任意一个

```java
package com.wnx.Stream.遍历匹配;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class Test {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(7, 6, 9, 3, 8, 2, 1);
        
        // 遍历输出符合条件的元素
        list.stream().filter(x -> x > 6).forEach(System.out::println);

     
        // 匹配任意（适用于并行流）
        Optional<Integer> findAny = list.parallelStream().filter(x -> x > 6).findAny();

        System.out.println("匹配任意一个值：" + findAny.get());



    }
}

```

# 16、allMatch()

- 是否包含符合特定条件的元素

```java
package com.wnx.Stream.遍历匹配;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class Test {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(7, 6, 9, 3, 8, 2, 1);
    

        // 是否包含符合特定条件的元素
        boolean anyMatch = list.stream().allMatch(x -> x < 6);

        System.out.println("是否存在小于6的值：" + anyMatch);


    }
}

```

# 17、Collectors

`Collectors`提供了一系列用于数据统计的静态方法：

计数：`count`
平均值：`averagingInt`、`averagingLong`、`averagingDouble`
最值：`maxBy`、`minBy`
求和：`summingInt`、`summingLong`、`summingDouble`
统计以上所有：`summarizingInt`、`summarizingLong`、`summarizingDouble`

```java

@Data
public class Person {
    private String name;  // 姓名
    private int salary; // 薪资
    private int age; // 年龄
    private String sex; //性别
    private String area;  // 地区

}

public class Test {
    public static void main(String[] args) {
        List<Person> personList = new ArrayList<Person>();
        personList.add(new Person("Tom", 8900, 23, "male", "New York"));
        personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 7800, 21, "female", "Washington"));

        // 求总数
        Long count = personList.stream()
                .collect(Collectors.counting());


        // 求平均工资
        Double average  = personList.stream()
                .collect(Collectors.averagingDouble(Person::getSalary));

        // 求工资之和
        Integer sum = personList.stream()
                .collect(Collectors.summingInt(Person::getSalary));


        // 一次性统计所有信息
        DoubleSummaryStatistics collect = personList.stream()
                .collect(Collectors.summarizingDouble(Person::getSalary));

        System.out.println("员工总数：" + count);
        System.out.println("员工平均工资：" + average);
        System.out.println("员工工资总和：" + sum);
        System.out.println("员工工资所有统计：" + collect);


    }
}

```

![](008-JDK8-Stream.assets/QQ%E6%88%AA%E5%9B%BE20211111101327.png)

# 18、concat()    distinct ()

- concat() 连接两个流
- distinct() 去掉两个流中重复的元素

```java
 public class Test {
    public static void main(String[] args) {
     String[] arr1 = { "a", "b", "c", "d" };
            String[] arr2 = { "d", "e", "f", "g" };
            Stream<String> stream1 = Stream.of(arr1);
            Stream<String> stream2 = Stream.of(arr2);

            // concat:合并两个流 distinct：去重
            List<String> newList = Stream
                    .concat(stream1, stream2)
                    .distinct()
                    .collect(Collectors.toList());
    }
}
```

# 19、sort()

sorted，中间操作。有两种排序：

- sorted()：自然排序，流中元素需实现Comparable接口
- sorted(Comparator com)：Comparator排序器自定义排序

```java
public class Test {
    public static void main(String[] args) {
        List<Person> personList = new ArrayList<Person>();

        personList.add(new Person("Sherry", 9000, 24, "female", "New York"));
        personList.add(new Person("Tom", 8900, 22, "male", "Washington"));
        personList.add(new Person("Jack", 9000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 8800, 26, "male", "New York"));
        personList.add(new Person("Alisa", 9000, 26, "female", "New York"));

        // 按工资升序排序（自然排序）
        List<String> newList  = personList.stream()
                .sorted(Comparator.comparing(Person::getSalary))
                .map(Person::getName)
                .collect(Collectors.toList());

        // 按工资倒序排序
        List<String> newList2  = personList.stream()
                .sorted(Comparator.comparing(Person::getSalary).reversed())
                .map(Person::getName)
                .collect(Collectors.toList());

        // 先按工资再按年龄升序排序
        List<String> newList3  = personList.stream()
                .sorted(Comparator.comparing(Person::getSalary)
                        .thenComparing(Comparator.comparing(Person::getAge).reversed()))
                .map(Person::getName).collect(Collectors.toList());

        // 先按工资再按年龄自定义排序（降序）

        List<String> newList4 = personList
                .stream()
                .sorted((p1, p2) -> {
                    if (p1.getSalary() == p2.getSalary()) {
                        return p2.getAge() - p1.getAge();
                    } else {
                        return p2.getSalary() - p1.getSalary();
                    }
                 })
                .map(Person::getName)
                .collect(Collectors.toList());


        System.out.println("按工资升序排序：" + newList);
        System.out.println("按工资降序排序：" + newList2);
        System.out.println("先按工资再按年龄升序排序：" + newList3);
        System.out.println("先按工资再按年龄自定义降序排序：" + newList4);
    }
}

```



# 20、join()

```java
        List<String> list = Arrays.asList("A", "B", "C");

        String string = list.stream().collect(Collectors.joining("-"));
        System.out.println("拼接后的字符串：" + string);
```

# 21、groupingBy()

```java
public class Test {
    public static void main(String[] args) {
        List<Person> personList = new ArrayList<Person>();
        personList.add(new Person("Tom", 8900, "male", "New York"));
        personList.add(new Person("Jack", 7000, "male", "Washington"));
        personList.add(new Person("Lily", 7800, "female", "Washington"));
        personList.add(new Person("Anni", 8200, "female", "New York"));
        personList.add(new Person("Owen", 9500, "male", "New York"));
        personList.add(new Person("Alisa", 7900, "female", "New York"));

        // 将员工按薪资是否高于8000分组
            Map<Boolean, List<Person>> part  = personList
                    .stream()
                    .collect(Collectors.partitioningBy(x -> x.getSalary() > 8000));

        // 将员工按性别分组
        Map<String, List<Person>> group = personList
                .stream()
                .collect(Collectors.groupingBy(Person::getSex));


        // 将员工先按性别分组，再按地区分组
        Map<String, Map<String, List<Person>>> group2  = personList
                .stream()
                .collect(Collectors.groupingBy(Person::getSex, Collectors.groupingBy(Person::getArea)));


        System.out.println("员工按薪资是否大于8000分组情况：" + part);
        System.out.println("员工按性别分组情况：" + group);
        System.out.println("员工按性别、地区：" + group2);

    }
}
```

# 22、distinct()

- 可对流数据进行去重操作

```java

    @Test
    public void test_Stream_distinct(){

        Map<String,Object> result1 = new HashMap<>();
        Map<String,Object> result2 = new HashMap<>();
        Map<String,Object> result3 = new HashMap<>();
        Map<String,Object> result4 = new HashMap<>();

        result1.put("DW","新丰江电厂");
        result2.put("DW","新丰江电厂");
        result3.put("DW","江门电厂");
        result4.put("DW","江门电厂");

        List<Map<String, Object>> resultList = Lists.newArrayList(result1, result2, result3, result4);

        String DW = resultList.stream()
                .map(item -> (String) item.get("DW"))
                .distinct()
                .collect(Collectors.joining(","));

        Assertions.assertEquals("新丰江电厂,江门电厂",DW);

    }

```

