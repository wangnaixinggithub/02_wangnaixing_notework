# JWT-JJWT-generate-Token-Form-Payload

```xml
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
```

```java
  	@Test
    public void generateTokenFromPayLoad(){
        //1.根据用户名生成Payload
        String username = "admin";
        Map<String,Object> payload = new HashMap<>();
        payload.put("sub",username);
        payload.put("created",new Date().getTime());
        payload.put("exp",DateUtils.addDays(new Date(),3).getTime());

        //2.提供一个密钥
        String secret = "WangNaiXing Is Great Engineer ";

        //2.根据payload生成token
        String token = Jwts.builder()
                .setClaims(payload)
                .signWith(SignatureAlgorithm.HS512, secret)
                .compact();

        System.out.println(token);
//eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNyZWF0ZWQiOjE2NTY1NTY3NjQ1MTEsImV4cCI6MTY1NjgxNTk2NH0.z954mBPerGY__EZu2xmh8ROLzyHWrnh1iv-K6fuV8sgJmgPbZxuOG_kSGuoXw9icXNnkhuZs5dzAkdn9_VN-2w
    }

```

