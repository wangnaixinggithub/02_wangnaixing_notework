# JWT-JJWT-generate-Payload-Form-UserName

```xml
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
```

-  payload的格式（用户名、创建时间、过期时间）：
- {"sub":"wang","created":1489079981393,"exp":1489684781}

```java
    @Test
    public void generatePayloadFormUserName(){
        String username = "admin";
        Map<String,Object> payload = new HashMap<>();
        payload.put("sub",username);
        payload.put("created",new Date().getTime());
        payload.put("exp",DateUtils.addDays(new Date(),3).getTime());

        System.out.println(payload);

    }
```

