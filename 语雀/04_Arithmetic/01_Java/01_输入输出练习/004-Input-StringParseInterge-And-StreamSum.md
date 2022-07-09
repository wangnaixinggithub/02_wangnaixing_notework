# Input-StringParseInterge-And-StreamSum

## 输入描述:

```
输入包括两个正整数a,b(1 <= a, b <= 10^9),输入数据有多组, 如果输入为0 0则结束输入
```

## 输出描述:

```
输出a+b的结果             
```

## 输入

```
1 5
10 20
0 0
```

## 输出

```
6
30
```



```java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            String numStr = scanner.nextLine();

            //1.将字符串数组 => 整形数组
            int[] numArray = parseStringToInt(numStr.split(" "));

            //2.输入终止条件
            if (Objects.equals(numArray[0],0) && Objects.equals(numArray[1],0)) {
                break;
            }

            //3.求和
            System.out.println(Arrays.stream(numArray).sum());


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