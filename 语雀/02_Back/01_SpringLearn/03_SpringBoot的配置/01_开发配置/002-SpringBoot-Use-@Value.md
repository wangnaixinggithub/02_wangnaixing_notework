# SpringBoot-@Value

# 1、在application.yaml中配置自定义属性

```yaml
book:
  bookId: 1
  bookName: SpringBoot入门到精通
  bookAuthor: 王乃醒
```

# 2、@Value

```java
@AllArgsConstructor
@NoArgsConstructor
@Component
@Data
public class Book implements Serializable {
    @Value("${book.bookId}")
    private Integer bookId;
    @Value("${book.bookName}")
    private String bookName;
    @Value("${book.bookAuthor}")
    private String bookAuthor;
}
```

```java
@SpringBootTest
class SpringWebfluxApplicationTests {
    @Autowired
    private Book book;
    @Test
    void contextLoads() {
        System.out.println(book); //Book(bookId=1, bookName=SpringBoot入门到精通, bookAuthor=王乃醒)
    }
}
```

