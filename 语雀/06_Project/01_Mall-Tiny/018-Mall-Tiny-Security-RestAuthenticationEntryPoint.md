# Mall-Tiny-Security-RestAuthenticationEntryPoint

# 1、Default Behavior

- 当SpringSecurity断定用户访问没有权限的接口时，交给`RestfulAccessDeniedHandler`处理。

# 2、implements AccessDeniedHandler

- 我们通过实现此接口，重写#handle()方法，来进行定制没有权限访问时，返回信息。

```java
package com.wnx.mall.tiny.security.component;

import cn.hutool.json.JSONUtil;
import com.wnx.mall.tiny.common.api.CommonResult;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.web.access.AccessDeniedHandler;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * 自定义返回结果：没有权限访问时
 */
public class RestfulAccessDeniedHandler implements AccessDeniedHandler{
    @Override
    public void handle(HttpServletRequest request,
                       HttpServletResponse response,
                       AccessDeniedException e) throws IOException, ServletException {

        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Cache-Control","no-cache");
        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json");
        
        response.getWriter().println(JSONUtil.parse(CommonResult.forbidden(e.getMessage())));
        
        response.getWriter().flush();
    }
}

```

# 

