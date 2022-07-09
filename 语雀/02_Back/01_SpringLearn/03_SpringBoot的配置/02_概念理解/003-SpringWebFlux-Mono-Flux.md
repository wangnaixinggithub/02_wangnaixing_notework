# SpringWebFlux- Mono Flux

# 1、Flux

```java
    @Test
    public void test() {
        List<Integer> needList = Lists.list(1, 2, 3, 4, 5, 6);
        //1。just方法直接声明
        Flux<Integer> f1 = Flux.just(1, 2, 3, 4, 5, 6);
        //2.通过List或者数组
        Flux<Integer> f2 = Flux.fromIterable(needList);
        //3.通过流
        Flux<Integer> f3 = Flux.fromStream(needList.stream());
    }
```

# 2、publisher

```java
    @Test
    public void test2() {
		//1.发布者发布消息
        List<Integer> needList = Lists.list(1, 2, 3, 4, 5, 6);
        Flux<Integer> publisher = Flux.fromIterable(needList);

        //2.订阅者拉取消息
        publisher.subscribe(item -> log.info(" item => {} ",item) );
    
    
```

```c
item => 1 
item => 2
item => 3
item => 4
item => 5 
item => 6 
```

# 3、Mono

```java
    @Test
    public void test3() {
        //只能发布一条消息
        User u = User.builder().userId(1L).userEmail("827376239@qq.com").userName("王乃醒").age(25).build();
        Mono<User> publisher = Mono.just(u);

        publisher.subscribe(item -> log.info(" item => {} ",item));

    }

```

```c#
 item => User(userId=1, userName=王乃醒, userPwd=null, userEmail=827376239@qq.com, age=25, dept=null) 
```

