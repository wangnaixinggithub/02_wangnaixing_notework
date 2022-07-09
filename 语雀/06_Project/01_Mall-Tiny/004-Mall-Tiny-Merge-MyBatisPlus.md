# Mall-Tiny-Merge-MyBatisPlus

## add Need Config

- 添加需要的配置

```java
package com.wnx.mall.tiny.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@EnableTransactionManagement
@MapperScan({"com.wnx.mall.tiny.modules.*.mapper"})
public class MyBatisConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
}

```

```yaml
#application.yaml
server:
  port: 8888

spring:
  application:
    name: mall-tiny
  profiles:
    active: dev
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher


mybatis-plus:
  mapper-locations: classpath:/mapper/**/*.xml
  global-config:
    db-config:
      id-type: auto
  configuration:
    auto-mapping-behavior: partial
    map-underscore-to-camel-case: true
```

```yaml
#application-dev.yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mall_tiny?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai&useSSL=false
    username: root
    password: root
  redis:
    host: localhost
    database: 0 
    port: 6379 
    password: 
    timeout: 3000ms 

logging:
  level:
    root: info
    com.wnx.mall: debug
```

