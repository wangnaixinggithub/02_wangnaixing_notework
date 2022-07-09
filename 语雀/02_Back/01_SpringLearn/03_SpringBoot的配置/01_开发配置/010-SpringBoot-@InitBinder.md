# SpringBoot-@InitBinder

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
public class Book implements Serializable {
    private Integer id;
    private String name;
    private Integer price;
}
```

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User implements Serializable {
    private Integer id;
    private String name;
    private Integer age;
}
```

```java
@ControllerAdvice
public class MyControllerAdvice {
    @InitBinder("u")
    public void a(WebDataBinder webDataBinder){
        webDataBinder.setFieldDefaultPrefix("u.");
    }
    @InitBinder("b")
    public void b(WebDataBinder webDataBinder){
        webDataBinder.setFieldDefaultPrefix("b.");
    }
}
```

```java
@Slf4j
@RestController
@RequestMapping("/book")
public class BookController {
     @PostMapping("/addBook")
    public String addBook(@ModelAttribute("u") User user,@ModelAttribute("b") Book book){
      log.info("User{}",user);
      log.info("Book{}",book);
      return "success";
    }
}
```

- 这样在前端创建表单时，就能用上前缀区分了，就可以避免一个表单中相同name名称问题，查看控制台，成功注入了！

<img src="121-SpringBoot%E5%AF%B9%E4%BA%8E%E8%A1%A8%E5%8D%95%E4%B8%AD%E7%9B%B8%E5%90%8Cname%E5%90%8D%E7%A7%B0%E6%8F%90%E4%BE%9B%E4%BA%86%E5%89%8D%E7%BC%80%E8%B5%8B%E5%80%BC%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.assets/image-20220327173607208.png" alt="image-20220327173607208"  />

```c
2022-03-27 17:37:44.462  INFO 2948 --- [nio-8080-exec-1] com.wnx.boot.controller.BookController   : UserUser(id=1, name=王乃醒, age=25)
2022-03-27 17:37:44.464  INFO 2948 --- [nio-8080-exec-1] com.wnx.boot.controller.BookController   : BookBook(id=1, name=JAVA入门到精通, price=300)
```

