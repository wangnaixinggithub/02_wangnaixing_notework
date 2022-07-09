# Bubble-Sort

```java
package com.wnx.algorithm.sort;

import java.util.Arrays;

public class Test {
    public static void main(String[] args) {
        int[] arr = new int[]{10,9,8,7,6,5,4,3,2,1};
        int length = arr.length;
        for (int i = 0; i < length -1; i++) {
            for (int j = 0; j < length -1; j++) {
                if (arr[j] > arr[j+1]){
                    int temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
        }

        Arrays.stream(arr).forEach(System.out::println);

    }

}
```

