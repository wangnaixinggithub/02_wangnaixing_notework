# JWT-JJWT-Get-Username-From-Token

```java
 	@Test
    public void getUserNameFormToken(){
        String token = "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImNyZWF0ZWQiOjE2NTY1NTY3NjQ1MTEsImV4cCI6MTY1NjgxNTk2NH0.z954mBPerGY__EZu2xmh8ROLzyHWrnh1iv-K6fuV8sgJmgPbZxuOG_kSGuoXw9icXNnkhuZs5dzAkdn9_VN-2w";
        String secret = "WangNaiXing Is Great Engineer ";

        //1.解析Token 得到payload.
        Claims claims = Jwts.parser()
                .setSigningKey(secret)
                .parseClaimsJws(token)
                .getBody();
        
        //2.从payload中得到用户名
        String username = claims.getSubject();
        
        
        System.out.println(username); //admin
    

    }
```

