# Mall-Tiny-Security-RestAuthenticationEntryPoint

# 1、Default Behavior

- 当SpringSecurity断定用户没有登录时，交给`AuthenticationEntryPoint`处理。

# 2、 implements AuthenticationEntryPoint

- 我们通过实现此接口，重写#commence()方法，来进行定制未登录返回信息。

```java
package com.wnx.mall.tiny.security.component;

import cn.hutool.json.JSONUtil;
import com.wnx.mall.tiny.common.api.CommonResult;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.AuthenticationEntryPoint;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * 自定义返回结果：未登录或登录过期
 * 
 */
public class RestAuthenticationEntryPoint implements AuthenticationEntryPoint {
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        
        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Cache-Control","no-cache");
        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json");
      
        response.getWriter().println(JSONUtil.parse(CommonResult.unauthorized(authException.getMessage())));
        
        response.getWriter().flush();
    }
}

```

