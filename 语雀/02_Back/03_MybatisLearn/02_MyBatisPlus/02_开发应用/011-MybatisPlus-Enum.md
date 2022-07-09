# MybatisPlus-XmlConfig-

# 1、枚举

```java
package com.wnx.mybatisplus.enums;

import com.baomidou.mybatisplus.annotation.EnumValue;
import lombok.AllArgsConstructor;
import lombok.Getter;

/**
 * @author by wangnaixing
 * @Description 比如性别字段：在数据库中1标识 男2标识女
 * @Date 2022/3/13 10:05
 */
@Getter
@AllArgsConstructor
public enum SexEnum {
    MALE(1, "男"),

    FEMALE(2, "女");

    @EnumValue
    private Integer sex;

    private String sexName;
}

```

# 2、Model

```java
package com.wnx.mybatisplus.model;

import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableLogic;
import com.wnx.mybatisplus.enums.SexEnum;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;


@Builder
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Serializable {

    @TableId("uid")
    private Long id;
    @TableField("user_name")
    private String name;
    private Integer age;
    private String email;
    @TableLogic
    private Integer isDeleted;
    private SexEnum sex;
}

```

## 2.1、插入时会把@EnumValue注解映射的值存入MySQL

```java
    @Test
    public void testSexEnum(){
        User user = new User();
        user.setName("Enum");
        user.setAge(20);
        user.setSex(SexEnum.MALE);
//INSERT INTO t_user ( username, age, sex ) VALUES ( ?, ?, ? )
//Parameters: Enum(String), 20(Integer), 1(Integer)
        userMapper.insert(user);
    }

```

## 2.2、查询时会得到枚举值

```java
User(id=5, name=ybc4, age=24, email=null, isDeleted=0, sex=MALE） //数据库中sex=1
User(id=5, name=ybc4, age=24, email=null, isDeleted=0, sex=FEMALE）//数据库中sex=2
```



