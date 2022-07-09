# Use-StreamSkip-doSum

## 输入描述:

```
输入数据包括多组。
每组数据一行,每行的第一个整数为整数的个数n(1 <= n <= 100), n为0的时候结束输入。
接下来n个正整数,即需要求和的每个正整数。
```

## 输出描述:

```
每组数据输出求和的结果                      
```

## 输入

```
4 1 2 3 4
5 1 2 3 4 5
0
```

## 输出

```
10
15
```



```java
package com.wnx.algorithm.input_output_learn;

import org.apache.commons.lang3.StringUtils;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Objects;
import java.util.Scanner;

/**
 * @program: backLearn
 * @ClassName Test
 * @description:
 * @author: 王乃醒
 * @create: 2022-06-11 11:46
 * @Version 1.0
 **/
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            String str = scanner.nextLine();
            //1.字符串数组=>整形数组
            int[] numberArr = parseStringToInt(str.split(" "));

            //2.循环终止条件
            if (Objects.equals(0,numberArr[0])) {
                break;
            }

            //3.过滤掉第一个，求和
            System.out.println(Arrays.stream(numberArr).skip(1).sum());

        }

    }

    /**
     * 将字符串数组转为整形数组
     * @param strArr 字符串数组
     * @return 转换后的整形数组
     */
    private static int[] parseStringToInt(String[] strArr){
        int[] numArr = new int[strArr.length];

        for (int i = 0; i < numArr.length; i++) {
                numArr[i] = Integer.parseInt(strArr[i]);
        }

        return numArr;

    }


}


```

