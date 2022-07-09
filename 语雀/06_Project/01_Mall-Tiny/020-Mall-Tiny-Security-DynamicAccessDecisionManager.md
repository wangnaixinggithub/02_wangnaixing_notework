# Mall-Tiny-Security-DynamicAccessDecisionManager

# 1、Default Behavior

- 默认，判断此认证主体是否有权进行访问资源，由`AccessDecisionManager` 决定。

# 2、implements AccessDecisionManager

- 实现`AccessDecisionManager`接口，重写`#decide()`方法。

```java
package com.wnx.mall.tiny.security.component;

import cn.hutool.core.collection.CollUtil;
import org.springframework.security.access.AccessDecisionManager;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.access.ConfigAttribute;
import org.springframework.security.authentication.InsufficientAuthenticationException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.GrantedAuthority;

import java.util.Collection;
import java.util.Iterator;

/**
 * 动态权限决策管理器，用于判断用户是否有访问权限
 *
 */
public class DynamicAccessDecisionManager implements AccessDecisionManager {

    @Override
    public void decide(Authentication authentication, Object object,
                       Collection<ConfigAttribute> configAttributes) throws AccessDeniedException, InsufficientAuthenticationException {
        
        // 1.当接口未被配置资源时直接放行
        if (CollUtil.isEmpty(configAttributes)) {
            return;
        }
        
        Iterator<ConfigAttribute> iterator = configAttributes.iterator();

        while (iterator.hasNext()) {
            
            ConfigAttribute configAttribute = iterator.next();
        
            //2.将访问所需资源或用户拥有资源进行比对
            String needAuthority = configAttribute.getAttribute();

            for (GrantedAuthority grantedAuthority : authentication.getAuthorities()) {
            
                if (needAuthority.trim().equals(grantedAuthority.getAuthority())) {
                    return;
                }
                
            }

        }

        throw new AccessDeniedException("抱歉，您没有访问权限");
    }

    
    @Override
    public boolean supports(ConfigAttribute configAttribute) {
        return true;
    }
    

    @Override
    public boolean supports(Class<?> aClass) {
        return true;
    }

}

```

