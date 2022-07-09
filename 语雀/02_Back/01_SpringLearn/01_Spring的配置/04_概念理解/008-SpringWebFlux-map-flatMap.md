#  SpringWebFlux-map-flatMap

# 1、map针对元素

```java
   @Test
    public void test4() {
        ArrayList<User> users = Lists.newArrayList(
                User.builder().userName("王乃醒1").userId(1L).build(),
                User.builder().userName("王乃醒2").userId(2L).build(),
                User.builder().userName("王乃醒2").userId(3L).build()
        );

        Flux<User> flux = Flux.fromIterable(users);
           Flux<Long> mapFlux = flux.map(user -> user.getUserId())
        
    }
```

# 2、flutMap针对流

```java
@Test
    public void test5() {
        ArrayList<User> users = Lists.newArrayList(
                User.builder().userName("王乃醒1").userId(1L).build(),
                User.builder().userName("王乃醒2").userId(2L).build(),
                User.builder().userName("王乃醒2").userId(3L).build()
        );

        Flux<User> flux = Flux.fromIterable(users);
        Flux<Long> mapFlux = flux.flatMap(item -> Flux.just(item.getUserId()));
    }
```

