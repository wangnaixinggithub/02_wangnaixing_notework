# Ip2region-Ip2regionSearcher

使用 `Ip2regionSearcher` 根据IP地址获取IP相关信息

```
官网：https://gitee.com/lionsoul/ip2region
```

# 1、memorySearch

```java
package com.wnx;

import lombok.extern.slf4j.Slf4j;
import net.dreamlu.mica.ip2region.core.Ip2regionSearcher;
import net.dreamlu.mica.ip2region.core.IpInfo;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
@Slf4j
class BackApplicationTests {

    @Autowired
    Ip2regionSearcher ip2regionSearcher;

    @Test
    void test_getIpInfo() {
        //IP IpInfo(cityId=null, country=中国, region=null, province=上海, city=上海, isp=联通, dataPtr=131903)
        String ip = "220.248.12.158";
        IpInfo ipInfo = ip2regionSearcher.memorySearch(ip);
          log.info("IP {}",ipInfo);
    }

    @Test
    public void test_getInfo2(){
        //IP IpInfo(cityId=null, country=美国, region=null, province=犹他, city=盐湖城, isp=null, dataPtr=173290)
        String ip = "67.220.90.13";
        IpInfo ipInfo = ip2regionSearcher.memorySearch(ip);
        log.info("IP {}",ipInfo);
    }


    @Test
    public void test_getInfo3(){
        //IP IpInfo(cityId=null, country=中国, region=null, province=广东, city=深圳, isp=阿里云, dataPtr=70663)
        String ip = "47.106.187.56";
        IpInfo ipInfo = ip2regionSearcher.memorySearch(ip);
        log.info("IP {}",ipInfo);
    }

}
```

# 2、btreeSearch

```java
package com.wnx;

import lombok.extern.slf4j.Slf4j;
import net.dreamlu.mica.ip2region.core.Ip2regionSearcher;
import net.dreamlu.mica.ip2region.core.IpInfo;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
@Slf4j
class BackApplicationTests {

    @Autowired
    Ip2regionSearcher ip2regionSearcher;

    @Test
    void test_getIpInfo() {
        //IP IpInfo(cityId=null, country=中国, region=null, province=上海, city=上海, isp=联通, dataPtr=131903)
        String ip = "220.248.12.158";
        IpInfo ipInfo = ip2regionSearcher.btreeSearch(ip);
          log.info("IP {}",ipInfo);
    }

    @Test
    public void test_getInfo2(){
        //IP IpInfo(cityId=null, country=美国, region=null, province=犹他, city=盐湖城, isp=null, dataPtr=173290)
        String ip = "67.220.90.13";
        IpInfo ipInfo = ip2regionSearcher.btreeSearch(ip);
        log.info("IP {}",ipInfo);
    }


    @Test
    public void test_getInfo3(){
        //IP IpInfo(cityId=null, country=中国, region=null, province=广东, city=深圳, isp=阿里云, dataPtr=70663)
        String ip = "47.106.187.56";
        IpInfo ipInfo = ip2regionSearcher.btreeSearch(ip);
        log.info("IP {}",ipInfo);
    }

    

}
```

# 3、binarySearch

```java
package com.wnx;

import lombok.extern.slf4j.Slf4j;
import net.dreamlu.mica.ip2region.core.Ip2regionSearcher;
import net.dreamlu.mica.ip2region.core.IpInfo;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
@Slf4j
class BackApplicationTests {

    @Autowired
    Ip2regionSearcher ip2regionSearcher;

    @Test
    void test_getIpInfo() {
        //IP IpInfo(cityId=null, country=中国, region=null, province=上海, city=上海, isp=联通, dataPtr=131903)
        String ip = "220.248.12.158";
        IpInfo ipInfo = ip2regionSearcher.binarySearch(ip);
          log.info("IP {}",ipInfo);
    }

    @Test
    public void test_getInfo2(){
        //IP IpInfo(cityId=null, country=美国, region=null, province=犹他, city=盐湖城, isp=null, dataPtr=173290)
        String ip = "67.220.90.13";
        IpInfo ipInfo = ip2regionSearcher.binarySearch(ip);
        log.info("IP {}",ipInfo);
    }


    @Test
    public void test_getInfo3(){
        //IP IpInfo(cityId=null, country=中国, region=null, province=广东, city=深圳, isp=阿里云, dataPtr=70663)
        String ip = "47.106.187.56";
        IpInfo ipInfo = ip2regionSearcher.binarySearch(ip);
        log.info("IP {}",ipInfo);
    }


}

```

