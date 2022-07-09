# Bean-Xml-depends-on-attr

```java
@Setter
public class Book implements Serializable {
      public Book() {
            System.out.println("Book被创建出来了..."); 
      }

      private String bookName;
      private String author;
      private String published;
        
}
@Setter
public class Dept implements Serializable {
    public Dept() {
        System.out.println("Dept被创建出来了...");
    }

    private Integer did;
    private String dname;
}
@Setter
public class Emp implements Serializable {
    public Emp() {
        System.out.println("Emp被创建出来了....");
    }

    private Integer eid;
    private String ename;
    private Dept dept;
}

```

```java
@Configuration
public class SpringConfig {

          //    告诉Spring，在创建Emp组件前，你先创建book组件和dept组件
    
    @Bean
    @DependsOn({"book","dept"})
    public Emp emp1(){
        Emp emp = new Emp();
        emp.setEid(1);
        emp.setEname("王乃醒");
        emp.setDept(null);
        return emp;

    }


    @Bean
    public Dept dept(){
        Dept dept = new Dept();
        dept.setDid(1);
        dept.setDname("开发一部");
        return dept;

    }
    @Bean
    public Book book(){
        Book book = new Book();
        book.setBookName("JAVA入门");
        book.setAuthor("王乃醒");
        book.setPublished("人民日报出版社");
        return book;
    }


}
```

```java
    @Log4j2
    @SpringJUnitConfig(SpringConfig.class)
    public class SpringJavaConfigTest {

        @Test
        public void test() throws SQLException {

        }
    }

```

```c#
Book被创建出来了...
Dept被创建出来了...
Emp被创建出来了....
```