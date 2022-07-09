# Define_Two_Dimensional_Array

- 定义二维数组，采用给数组元素赋值的方法

```java
    @Test
    public void test_define_two_dimensional_array(){
            int[][] data = new int[2][3];

            data[0] = new int[]{1,2,3};
            data[1] = new int[]{4,5,6};

        Assertions.assertEquals("[1, 2, 3]",Arrays.toString(data[0]));
        Assertions.assertEquals("[4, 5, 6]",Arrays.toString(data[1]));

    }
```

- 定义二维数组，采用给每一个元素赋值的方法。

```java
    @Test
    public void test_define_two_dimensional_array(){
        
        	
            int[][] data = new int[2][3];
            
        	//1.给data元素赋值
        	data[0][0] = 1;
            data[0][1] = 2;
            data[0][2] = 3;

            data[1][0] = 4;
            data[1][1] = 5;
            data[1][2] = 6;

        Assertions.assertEquals("[1, 2, 3]",Arrays.toString(data[0]));
        Assertions.assertEquals("[4, 5, 6]",Arrays.toString(data[1]));

    }
```

