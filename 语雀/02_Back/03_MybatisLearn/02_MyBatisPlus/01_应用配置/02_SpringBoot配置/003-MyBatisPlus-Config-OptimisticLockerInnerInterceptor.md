# MyBatisPlus-Config-OptimisticLockerInnerInterceptor

```java
@Configuration
public class MyBatisConfig {

 
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));

        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());

      
      
        return interceptor;
    }
}
```

```java
@Data
public class Shop {

    private Long id;
    private String name;
    private Integer price;
    @Version // 声明版本号属性
    private Integer version;
}
```

