# JWT-JJWT-Get-Payload-From-Token

```xml
		 <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
```

- 从Token中解析出来负载payload信息。

```java
    @Test
    public void getPayloadFromToken(){
        
        //1.假设这是客户端传到服务端的Token
        String token = "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNyZWF0ZWQiOjE2NTY1NTY3NjQ1MTEsImV4cCI6MTY1NjgxNTk2NH0.z954mBPerGY__EZu2xmh8ROLzyHWrnh1iv-K6fuV8sgJmgPbZxuOG_kSGuoXw9icXNnkhuZs5dzAkdn9_VN-2w";
        
        //2.使用加密的密钥
        String secret = "WangNaiXing Is Great Engineer ";

        Claims payload = Jwts.parser()
                .setSigningKey(secret)
                .parseClaimsJws(token).getBody();

        System.out.println(payload); //{sub=admin, created=1656556764511, exp=1656815964}
    }

```

