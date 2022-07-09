# JWT-Parse-Use-Nimbus-Jose-Jwt

> nimbus-jose-jwt 使用他可以生成或者解析对称加密或者非对称加密的的JWT. JWS是JWT规范的落地实现。

# 1.Need Config

```xml
        <!--JWT解析库-->
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>nimbus-jose-jwt</artifactId>
            <version>8.16</version>
        </dependency>
        <!--Spring Security RSA工具类-->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-rsa</artifactId>
            <version>1.0.7.RELEASE</version>
        </dependency>
```

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root

mybatis:
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**/mapper/*.xml

```

# 2、User It

```java
package com.wnx.mall.tiny.service;

import com.nimbusds.jose.JOSEException;
import com.nimbusds.jose.KeyLengthException;
import com.nimbusds.jose.jwk.RSAKey;
import com.wnx.mall.tiny.domain.PayloadDto;

import java.text.ParseException;


public interface  JwtTokenService {

    /**
     * 使用HMAC算法生成TOken
     * @param payloadStr
     * @param secret
     * @return
     */
    String generateTokenByHMAC(String payloadStr,String secret) throws JOSEException;

    /**
     * 使用HMAC验证Token
     * @param token
     * @param secret
     * @return
     */
    PayloadDto verifyTokenByHMAC(String token,String secret) throws ParseException, JOSEException;

    /**
     * 使用RSA算法生成Token
     * @param payloadStr
     * @param rsaKey
     * @return
     */
    String generateTokenByRSA(String payloadStr,RSAKey rsaKey) throws JOSEException;

    /**
     * 使用RSA算法验证token
     * @param token
     * @param rsaKey
     * @return
     */
    PayloadDto verifyTokenByRSA(String token,RSAKey rsaKey) throws ParseException, JOSEException;


    /**
     * 获取默认的payload
     * @return
     */
    PayloadDto getDefaultPayloadDto();

    /**
     * 获取默认的RSAKey
     */
    RSAKey getDefaultRSAKey();



}

```





```java
@Service
public class JwtTokenServiceImpl implements JwtTokenService {

    @Override
    public String generateTokenByHMAC(String payloadStr, String secret) throws JOSEException {
        
        //1.创建JWS头，设置签名算法和类型
        JWSHeader jwsHeader = new JWSHeader
                .Builder(JWSAlgorithm.HS256)
                .type(JOSEObjectType.JWT)
                .build();
       
        //2.将负载信息封装到Payload中
        Payload payload = new Payload(payloadStr);
        
        //3.创建JWS对象
        JWSObject jwsObject = new JWSObject(jwsHeader,payload);
        
        //4.创建HMAC签名器
        JWSSigner jwsSigner = new MACSigner(secret);
        
        //5.签名
        jwsObject.sign(jwsSigner);
      
        return jwsObject.serialize();


    }

    @Override
    public PayloadDto verifyTokenByHMAC(String token, String secret) throws ParseException, JOSEException {
        //从token中解析JWS对象
        JWSObject jwsObject = JWSObject.parse(token);
      
        //创建HMAC验证器
        JWSVerifier jwsVerifier = new MACVerifier(secret);
        
        if (!jwsObject.verify(jwsVerifier)){
            throw new JwtInvalidException("token签名不合法");
        }
        String payload = jwsObject.getPayload().toString();
  
        
        PayloadDto payloadDto = JSONUtil.toBean(payload, PayloadDto.class);
  
        if (payloadDto.getExp() < new Date().getTime()){
            throw new JwtExpiredException("token已过期！");
        }
        
        return payloadDto;
    }

    @Override
    public String generateTokenByRSA(String payloadStr, RSAKey rsaKey) throws JOSEException {
        //1.创建JWS头，设置签名算法和类型
        JWSHeader jwsHeader = new JWSHeader
                .Builder(JWSAlgorithm.RS256)
                .type(JOSEObjectType.JWT)
                .build();
        
        //2.将负载信息封装到payload中
        Payload payload = new Payload(payloadStr);
        
        
        //3.创建JWS对象
        JWSObject jwsObject = new JWSObject(jwsHeader,payload);
        
        //4.创建RSA签名器
        JWSSigner jwsSigner = new RSASSASigner(rsaKey,true);
        
        //5.签名
        jwsObject.sign(jwsSigner);
        
        return jwsObject.serialize();
    }

    @Override
    public PayloadDto verifyTokenByRSA(String token, RSAKey rsaKey) throws ParseException, JOSEException {
        //从token中解析JWS对象
        JWSObject jwsObject = JWSObject.parse(token);
        RSAKey publicRsaKey = rsaKey.toPublicJWK();
        
        //使用RSA公钥创建RSA验证器
        JWSVerifier jwsVerifier = new RSASSAVerifier(publicRsaKey);
        
        if (!jwsObject.verify(jwsVerifier)){
            throw new JwtInvalidException("token签名不合法！");
        }
        String payload = jwsObject.getPayload().toString();
        PayloadDto payloadDto = JSONUtil.toBean(payload, PayloadDto.class);
        
        if (payloadDto.getExp() < new Date().getTime()){
            throw new JwtExpiredException("token已过期！");
        }
        return payloadDto;
    }

    @Override
    public PayloadDto getDefaultPayloadDto() {
        Date now = new Date();
        Date exp = DateUtil.offsetSecond(now,60*60);
        return PayloadDto.builder()
                .sub("macro")
                .iat(now.getTime())
                .exp(exp.getTime())
                .jti(UUID.randomUUID().toString())
                .username("macro")
                .authorities(CollUtil.toList("ADMIN"))
                .build();

    }

    @Override
    public RSAKey getDefaultRSAKey() {
        //从classpath下获取RSA密钥对
        KeyStoreKeyFactory keyStoreKeyFactory = new KeyStoreKeyFactory(new ClassPathResource("jwt.jks"), "123456".toCharArray());
        KeyPair keyPair = keyStoreKeyFactory.getKeyPair("jwt", "123456".toCharArray());
        //获取RSA公钥
        RSAPublicKey publicKey = (RSAPublicKey) keyPair.getPublic();
        //获取RSA私钥
        PrivateKey privateKey = keyPair.getPrivate();

        return new RSAKey.Builder(publicKey).privateKey(privateKey).build();
    }
}

```

```java
package com.wnx.mall.tiny.domain;

import io.swagger.annotations.ApiModelProperty;
import lombok.Builder;
import lombok.Data;
import lombok.EqualsAndHashCode;

import java.util.List;
@Data
@EqualsAndHashCode(callSuper = false)
@Builder
public class PayloadDto {
    @ApiModelProperty("主题")
    private String sub;
    @ApiModelProperty("签发时间")
    private Long iat;
    @ApiModelProperty("过期时间")
    private Long exp;
    @ApiModelProperty("JWT的ID")
    private String jti;
    @ApiModelProperty("用户名称")
    private String username;
    @ApiModelProperty("用户拥有的权限")
    private List<String> authorities;
}

```

- 自定义异常

```java
package com.wnx.mall.tiny.exception;

public class JwtExpiredException extends RuntimeException{
    public JwtExpiredException(String message) {
        super(message);
    }
}
```

```java
package com.wnx.mall.tiny.exception;

public class JwtInvalidException extends RuntimeException{
    public JwtInvalidException(String message) {
        super(message);
    }
}
```





