# @Condition 条件注解

## 1、一个接口实现了两个Bean



```java
package com.wnx.spring.condition.model;

public interface ShowCmd {

    /**
     * 打印命令
     */
    public String printCommand();
}
```

```java
package com.wnx.spring.condition.model;

public class LinuxCmd implements ShowCmd{
    @Override
    public String printCommand() {
        return "ls";
    }
}

```

```java
package com.wnx.spring.condition.model;

public class WindowCmd  implements ShowCmd{

    @Override
    public String printCommand() {
        return "dir";
    }
}
```

## 2、定义创建Bean的条件

```java
package com.wnx.spring.condition.handler;

import org.apache.commons.lang3.StringUtils;
import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

public class LinuxCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        return StringUtils.trimToEmpty(context.getEnvironment().getProperty("os.name")).toLowerCase().contains("linux");
    }
}

```

```java
package com.wnx.spring.condition.handler;

import org.apache.commons.lang3.StringUtils;
import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

public class WindowCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        return StringUtils.trimToEmpty(context.getEnvironment().getProperty("os.name")).toLowerCase().contains("windows");
    }
}

```

## 3、条件配置

```java
package com.wnx.spring.config;


import com.wnx.spring.condition.handler.LinuxCondition;
import com.wnx.spring.condition.handler.WindowCondition;
import com.wnx.spring.condition.model.LinuxCmd;
import com.wnx.spring.condition.model.WindowCmd;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Conditional;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = "com.wnx.spring")
public class SpringConfig {


    @Bean("showCmd")
    @Conditional(WindowCondition.class)
    public WindowCmd winCmd(){
        return new WindowCmd();
    }

    @Bean("showCmd")
    @Conditional(LinuxCondition.class)
    public LinuxCmd linuxCmd(){
        return new LinuxCmd();
    }

}

```

## 4、触发条件才实例化Bean

```java
package com.wnx.spring;

import com.wnx.spring.condition.model.ShowCmd;
import com.wnx.spring.config.SpringConfig;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.junit.jupiter.SpringJUnitConfig;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/4/23 14:41
 */

@SpringJUnitConfig(classes = SpringConfig.class)
public class SpringTest {
      @Autowired
      private ShowCmd showCmd;

    @Test
    public void test() {
        System.out.println(showCmd.printCommand());        //dir
    }


}
```