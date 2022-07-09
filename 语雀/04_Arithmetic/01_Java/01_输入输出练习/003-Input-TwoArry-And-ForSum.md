# Input-GetDoTime-And-ForSum

- 定义二维数组来处理做多少次的事情
- 二维数组的遍历求和

## 输入描述:

```
输入第一行包括一个数据组数t(1 <= t <= 100)
接下来每行包括两个正整数a,b(1 <= a, b <= 1000)
```

## 输出描述:

```
输出a+b的结果               
```

## 输入

```
2
1 5
10 20
```

## 输出

```
6
30
```





```java
package com.wnx.algorithm.input_output_learn;

import org.apache.commons.lang3.StringUtils;

import java.util.ArrayList;
import java.util.Arrays;
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
        
        //1.读取需要进行几次求和运算。
        int count = Integer.parseInt(scanner.next());
        int[][] data = new int[count][2];

        //2.将用户输入值进行赋值
        for (int i = 0; i < count; i++) {
            data[i][0] = scanner.nextInt();
            data[i][1] = scanner.nextInt();
        }

        //3.做求和计算
        int[] answer = calculateSum(data);

        //4.打印求和结果
      Arrays.stream(answer).forEach(System.out::println);

    }

    /**
     * 对二维数组元素进行求和操作
     * @param data 求和的二维数组
     * @return 封装求和结果的一维数组
     */
    private static int[]  calculateSum(int data [][]){
        int[] answer = new int[data.length];
        for (int i = 0; i < data.length; i++) {
            answer[i] += data[i][0] +data[i][1];
        }
        return answer;
    }


}



```

