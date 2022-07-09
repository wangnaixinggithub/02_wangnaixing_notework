# MyBatisPlus-Auto-Fill-Value

# 1、implements MetaObjectHandler

```java
package com.wnx.mall.tiny.handler;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

/**
 * 填充器
 *
 * @author nieqiurong 2018-08-10 22:59:23.
 */
@Slf4j
@Component
public class MyMetaObjectHandler  implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill ....");
        this.strictInsertFill(metaObject,"operator",String.class,"wangnaixing");
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill ....");
        this.strictUpdateFill(metaObject,"operator",String.class,"huangjieying");
    }
}

```

# 2、` @TableField(fill = FieldFill.INSERT_UPDATE)`

```java
@Data
@EqualsAndHashCode(callSuper = false)
@TableName("user")
@AllArgsConstructor
@ApiModel(value="User对象", description="")
public class User implements Serializable {

    private static final long serialVersionUID=1L;

    @ApiModelProperty(value = "主键ID")
    private Long id;

    @ApiModelProperty(value = "姓名")
    private String name;

    @ApiModelProperty(value = "年龄")
    private Integer age;

    @ApiModelProperty(value = "邮箱")
    private String email;

    @ApiModelProperty(value = "操作员")
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private String operator;


}

```

# 3、Validation Test

- 仅仅在值为`NuLL`值的时候 才会自动填入！

```java
package com.wnx.mall.tiny;

import com.wnx.mall.tiny.modules.test.mapper.UserMapper;
import com.wnx.mall.tiny.modules.test.model.User;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

import javax.annotation.Resource;

@Slf4j
@SpringBootTest
public class AutoFillTest {

    @Resource
    private UserMapper userMapper;


    @Test
    public void test() {
        User user = new User(null, "Tom", 1, "tom@qq.com", null);
        
        //1.插入值填充
        userMapper.insert(user);
       
        log.info("query user:{}", userMapper.selectById(user.getId()));
        User beforeUser = userMapper.selectById(1L);
        log.info("before user:{}", beforeUser);
       
        //2.更新值填充
        beforeUser.setAge(12);
        beforeUser.setOperator(null);
        userMapper.updateById(beforeUser);
        log.info("query user:{}", userMapper.selectById(1L));
    }
}

```

![image-20220113093534873](D:/%E5%9B%BE%E7%89%87%E7%BB%9F%E4%B8%80%E4%BF%9D%E7%AE%A1%E5%A4%84/image-20220113093534873.png)