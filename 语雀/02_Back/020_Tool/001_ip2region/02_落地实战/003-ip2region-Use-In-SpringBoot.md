# ip2region-Use-In-SpringBoot

```java
package com.wnx.controller;

import com.wnx.api.CommonResult;
import com.wnx.utils.IpUtil;
import net.dreamlu.mica.ip2region.core.Ip2regionSearcher;
import net.dreamlu.mica.ip2region.core.IpInfo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;

import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

@RestController
public class BackController {

    
    @Autowired
    Ip2regionSearcher ip2regionSearcher;

    @GetMapping("/ipaddress")
    public CommonResult ipTest(HttpServletRequest request) throws IOException {

        // 1、获取到请求的ip地址
        String ipAddr = IpUtil.getIpAddr(request);


        // 2、根据ip地址获取到IP信息
        IpInfo ipInfo = ip2regionSearcher.memorySearch(ipAddr);



        return  CommonResult.success(ipInfo);
    }




}
```

