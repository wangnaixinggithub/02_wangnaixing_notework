# SpringBoot-@PropertySource

# 1、@PropertySource

- 在resouces目录下，提供book.properties文件

```properties
book.bookId=2
book.bookName=myBookName
book.bookAuthor=wangnaixing
```

- 使用@PropertySource注解，指明配置文件是类路径下的book.properties文件，通过@Value注解，用占位符取值的方法完成属性的properties文件的映射。

```java
@AllArgsConstructor
@NoArgsConstructor
@Component
@Data
@PropertySource("classpath:book.properties")
public class Book implements Serializable {
    @Value("${book.bookId}")
    private Integer bookId;
    @Value("${book.bookName}")
    private String bookName;
    @Value("${book.bookAuthor}")
    private String bookAuthor;
}
```

- 成功通过properties文件的属性注入到SpringBoot的Book组件中

```java
@Slf4j
@SpringBootTest
class SpringWebfluxApplicationTests {
    @Autowired
    private Book book;
    @Test
    void contextLoads() {
        log.info("{}",book);
         //Book(bookId=1, bookName=SpringBoot入门到精通, bookAuthor=王乃醒)
    }
}
```

