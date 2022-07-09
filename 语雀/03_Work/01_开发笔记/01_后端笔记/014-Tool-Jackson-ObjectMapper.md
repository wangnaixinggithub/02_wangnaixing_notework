# Tool-Jackson-ObjectMapper



# 1、`objectMapper#writeValueAsString()`

```java
 @SneakyThrows
    @Test
    public void test2(){
        ObjectMapper objectMapper = new ObjectMapper();
        
        User user = new User(1L,"王乃醒大帅哥！");

        String jsStr = objectMapper.writeValueAsString(user);

        System.out.println(jsStr); //{"userId":1,"userName":"王乃醒大帅哥！"}

    }
```

# 2、`objectMapper#readValue()`

```java
    @SneakyThrows
    @Test
    public void test3(){
        ObjectMapper objectMapper = new ObjectMapper();
        String jsonStr = "{\"userId\":1,\"userName\":\"王乃醒大帅哥！\"}";
        
        User user = objectMapper.readValue(jsonStr, User.class);
        
        System.out.println(user); //User(userId=1, userName=王乃醒大帅哥！)
    }
```

