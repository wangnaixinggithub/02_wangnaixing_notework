# MyBatisPlus-Use-Enum-Value

# 1、One Way  implements IEnum

```java
package com.wnx.mall.tiny.enums;

import com.baomidou.mybatisplus.core.enums.IEnum;
import lombok.Getter;


@Getter
public enum  GradeEnum implements IEnum<Integer> {

 	PRIMARY(1, "小学"),
    SECONDORY(2, "中学"),
    HIGH(3, "高中");
    
    private final int code;
    private final String desc;


    GradeEnum(int code,String desc) {
        this.code = code;
        this.descp = desc;
    }
    
    private final int value;
    private final String desc;

 

    @Override
    public Integer getValue() {
        return code;
    }
}

```



# 2、Two Way Use @EnumValue

```java
package com.wnx.mall.tiny.enums;

import com.baomidou.mybatisplus.annotation.EnumValue;
import lombok.Getter;

@Getter
public enum  GradeEnum {
    
    PRIMARY(1, "小学"),
    SECONDORY(2, "中学"),
    HIGH(3, "高中");

    @EnumValue
    private final int code;
    private final String desc;


    GradeEnum(int code,String desc) {
        this.code = code;
        this.descp = desc;
    }
}

```

# 3、Need Config

- 配置枚举包位置

```yaml
spring:
  datasource:
    driver-class-name: org.h2.Driver
    schema: classpath:db/schema-h2.sql
    data: classpath:db/data-h2.sql
    url: jdbc:h2:mem:test
    username: root
    password: test


logging:
  level:
    root: info
    com.wnx.mall.tiny: debug


mybatis-plus:
  type-enums-package: com.wnx.mall.tiny.enums
  configuration:
    default-enum-type-handler: org.apache.ibatis.type.EnumOrdinalTypeHandler
```

