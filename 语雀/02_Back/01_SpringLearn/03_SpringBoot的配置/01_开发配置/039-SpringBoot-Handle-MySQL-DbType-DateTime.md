# SpringBoot-Handle-MySQL-DbType-DateTime

# 1、No Handle Before

![image-20220107105844295](https://s2.loli.net/2022/01/07/p8NWq5Ow4ecixZX.png)

```java
@Data
public class UmsAdmin implements Serializable {
    
    private Long id;

    private String username;

    private String password;

    @ApiModelProperty(value = "头像")
    private String icon;

    @ApiModelProperty(value = "邮箱")
    private String email;

    @ApiModelProperty(value = "昵称")
    private String nickName;

    @ApiModelProperty(value = "备注信息")
    private String note;
    
    @ApiModelProperty(value = "创建时间")
    private Date createTime;
    
    @ApiModelProperty(value = "最后登录时间")
    private Date loginTime;

    @ApiModelProperty(value = "帐号启用状态：0->禁用；1->启用")
    private Integer status;

}
```

```java
@Api(tags = "JsonController", description = "SpringBootJSON测试")
@Controller
@RequestMapping("/json")
public class JsonController {
    
    @Resource
    private UmsAdminService adminService;

    @GetMapping("findById/{id}")
    @ResponseBody
    public UmsAdmin findById(@PathVariable(name = "id")Long id) {
       UmsAdmin umsAdmin = adminService.findById(id);
        System.out.println(umsAdmin);
        return umsAdmin;
    }
}

```

```java
{
  "id": 1,
  "username": "test",
  "password": "$2a$10$NZ5o7r2E.ayT2ZoxgjlI.eJ6OEYqjH7INR/F.mXDbjZJi9HF0YCVG",
  "icon": "http://macro-oss.oss-cn-shenzhen.aliyuncs.com/mall/images/20180607/timg.jpg",
  "email": "test@qq.com",
  "nickName": "测试账号",
  "note": null,
  "createTime": "2018-09-29T05:55:30.000+0000",
  "loginTime": "2018-09-29T05:55:39.000+0000",
  "status": 1
}
```

可看到时间默认以`2018-09-29T05:55:30.000+0000`的形式输出，如果想要改变这个默认行为，我们有三种如下解决手段！

# 2、Handle Three Way

## 1、Way01-Customer ObjectMapper

```java
package com.wnx.mall.tiny.config;

import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.text.SimpleDateFormat;

@Configuration
public class JacksonConfig {
    @Bean
    public ObjectMapper getObjectMapper(){
        ObjectMapper mapper = new ObjectMapper();
        mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));
        return mapper;
    }
}

```

## 2、Way02-Property-Add-`@JsonFormat`

```java
@Data
public class UmsAdmin implements Serializable {
    private Long id;

    private String username;

    private String password;

    @ApiModelProperty(value = "头像")
    private String icon;

    @ApiModelProperty(value = "邮箱")
    private String email;

    @ApiModelProperty(value = "昵称")
    private String nickName;

    @ApiModelProperty(value = "备注信息")
    private String note;
   
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    @ApiModelProperty(value = "创建时间")
    private Date createTime;
    
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    @ApiModelProperty(value = "最后登录时间")
    private Date loginTime;

    @ApiModelProperty(value = "帐号启用状态：0->禁用；1->启用")
    private Integer status;


   
}
```

## 3、Update YAML

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss

mybatis:
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**/mapper/*.xml

```

# 3、Handle After

![image-20220107110729804](https://s2.loli.net/2022/01/07/WJifaNAsbzGmltF.png)

