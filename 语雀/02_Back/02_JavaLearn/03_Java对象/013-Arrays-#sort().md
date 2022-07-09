# Arrays-#sort()

- 可以对字符数组进行排序

```java
    @Test
    public void test_sort_strArray(){
        String[] str  = "b c d a b c".split(" ");
        Arrays.sort(str);

        Assertions.assertEquals("[a, b, b, c, c, d]",Arrays.toString(str));
    }
```

- 可以对数组数组进行排序

```java
    @Test
    public void test_sort_numArray(){
        int[] numArr = new int[]{10,6,5,1,3};
        Arrays.sort(numArr);
        Assertions.assertEquals("[1, 3, 5, 6, 10]",Arrays.toString(numArr));

    }
```

