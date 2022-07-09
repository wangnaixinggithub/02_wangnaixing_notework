

# MyBatisPlus-解决列名和属性名不一致

## 1、@TableId

> 方式1：让Model属性ID和表记录主键列名一致，加上[@TableId ]() 

```java
package com.wnx.mybatisplus.model;

import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/10 23:29
 */
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Serializable {
    @TableId
    private Long uid;
    private String name;
    private Integer age;
    private String email;
}

```

> 方式2：指定@TableId的value属性值等于表记录主键列名一致，这个时候POJO属性名随意。

```java
package com.wnx.mybatisplus.model;

import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/10 23:29
 */
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Serializable {
    @TableId("uid")
    private Long id;
    private String name;
    private Integer age;
    private String email;
}

```

## 2、 @TableFiled

> 让POJO属性字段和记录列名匹配

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Serializable {
    @TableId("u_id")
    private Long id;
    @TableFiled("u_name")
    private String name;
    @TableFiled("u_age")
    private Integer age;
    @TableFiled("u_email")
    private String email;
}
```

