# Use-JavaStream-calcSum

# 1、BigDecimal

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Long bookId;
    private String bookName;
    private BigDecimal bookPrice;
}

```

```java
    @Test
    public void bigDecimal_Prop_getSum_Test(){
        List<Book> bookList = new ArrayList<>();

        bookList.add(
                Book.builder().bookId(1L).bookPrice(new BigDecimal("172.5")).bookName("JAVA").build()
        );

        bookList.add(
                Book.builder().bookId(2L).bookPrice(new BigDecimal("272.5")).bookName("VUE").build()
        );

        bookList.add(
                Book.builder().bookId(3L).bookPrice(new BigDecimal("372.5")).bookName("SpringBoot").build()
        );


        BigDecimal totalPrice = bookList.stream()
                .map(Book::getBookPrice)
                .reduce(BigDecimal.ZERO, BigDecimal::add);

        log.info("SUM => {}",totalPrice);

        //17:02:56.156 [main] INFO com.wnx.mall.test6.BigDecimalTest - SUM => 817.5

    }
```

# 2、double

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Long bookId;
    private String bookName;
    private Double bookPrice;
}
```

```java
   @Test
    public void double_Prop_getSum_Test(){
        List<Book> bookList = new ArrayList<>();

        bookList.add(
                Book.builder().bookId(1L).bookPrice(172.5).bookName("JAVA").build()
        );

        bookList.add(
                Book.builder().bookId(2L).bookPrice(272.5).bookName("VUE").build()
        );

        bookList.add(
                Book.builder().bookId(3L).bookPrice(372.5).bookName("SpringBoot").build()
        );


        Double totalPrice = bookList.stream()
                .mapToDouble(Book::getBookPrice)
                .sum();

        log.info("SUM => {}",totalPrice);

        //17:05:55.502 [main] INFO com.wnx.mall.test6.BigDecimalTest - SUM => 817.5
    }
```

# 3、Long

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Long bookId;
    private String bookName;
    private Long bookPrice;
}
```

```java
   @Test
    public void long_Prop_getSum_Test(){
        List<Book> bookList = new ArrayList<>();

        bookList.add(
                Book.builder().bookId(1L).bookPrice(172L).bookName("JAVA").build()
        );

        bookList.add(
                Book.builder().bookId(2L).bookPrice(272L).bookName("VUE").build()
        );

        bookList.add(
                Book.builder().bookId(3L).bookPrice(372L).bookName("SpringBoot").build()
        );


        Double totalPrice = bookList.stream()
                .mapToDouble(Book::getBookPrice)
                .sum();

        log.info("SUM => {}",totalPrice);

        //17:08:22.511 [main] INFO com.wnx.mall.test6.BigDecimalTest - SUM => 816.0
    }


```

