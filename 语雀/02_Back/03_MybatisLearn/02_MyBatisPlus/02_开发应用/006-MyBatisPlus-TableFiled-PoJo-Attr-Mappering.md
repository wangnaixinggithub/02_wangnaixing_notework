# MyBatisPlus处理属性名和字段名不一致

# 1、默认自带了驼峰转换

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
    /**
     * 可以设置类型根据ID自增的，设置完了之后一定确保数据库也开启了使用自增策略！
     */
    @TableId("uid")
    private Long id;
    //Plus 默认配置了驼峰命名，和字段user_name匹配
    private String userName;
    private Integer age;
    private String email;
}

```

# 2、@TableField("user_name")

```java
package com.wnx.mybatisplus.model;

import com.baomidou.mybatisplus.annotation.TableField;
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
    /**
     * 使用@TableField 进行属性和字段映射！
     */
    @TableField("user_name")
    private String name;
    private Integer age;
    private String email;
}

```

