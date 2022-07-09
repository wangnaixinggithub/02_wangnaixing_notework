# SpringBoot-Async-Invoke

> 目的：异步调用，提升接口的吞吐量。

# 1、Need Config

```java

@Configuration
public class AsyncPoolConfig {

    @Bean
    public ThreadPoolTaskExecutor asyncThreadPoolTaskExecutor(){
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        //设置线程池核心线程数量
        executor.setCorePoolSize(20);
        //设置线程池维护的线程的最大数量
        executor.setMaxPoolSize(200);
        //设置缓存队列容量
        executor.setQueueCapacity(25);
        //设置超出核心线程数外的线程在空闲时候的最大存活时间，默认60秒
        executor.setKeepAliveSeconds(200);
        //设置线程名前缀
        executor.setThreadNamePrefix("asyncThread");
        //设置是否等待所有线程执行完毕才关闭线程池？
        executor.setWaitForTasksToCompleteOnShutdown(true);
        executor.setAwaitTerminationSeconds(60);
        //设置没有线程可以被使用的处理策略，拒绝。
        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        executor.initialize();
        return executor;
    }

}

```

```java
@EnableAsync
@SpringBootApplication
public class SpringBootAsyncApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootAsyncApplication.class, args);
    }

}
```

# 2、Use It

> 无返回值和有返回值的情况，以及同步方法对比。都是睡2秒

```java
package com.wnx.mall.tiny.service.impl;

import com.wnx.mall.tiny.service.TestService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.scheduling.annotation.Async;
import org.springframework.scheduling.annotation.AsyncResult;
import org.springframework.stereotype.Service;

import java.util.concurrent.Future;
import java.util.concurrent.TimeUnit;
@Slf4j
@Service
public class TestServiceImpl implements TestService {

    @Async("asyncThreadPoolTaskExecutor")
    @Override
    public void asyncMethod() {
        sleep();
        log.info("异步方法内部线程名称：{}",Thread.currentThread().getName());

    }

    @Async("asyncThreadPoolTaskExecutor")
    @Override
    public Future<String> asyncMethod2() {
        sleep();
        log.info("异步方法内部线程名称：{}",Thread.currentThread().getName());
        return new AsyncResult<>("hello async");
    }

    @Override
    public void syncMethod() {
        sleep();

    }

    private void sleep()  {
        try {
            TimeUnit.SECONDS.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}

```

# 3、Validation Test

```java

@Slf4j
@Api(tags = "TestController", description = "异步调用测试")
@RestController
public class AsyncTestController {
    @Resource
    private TestService testService;

    @ApiOperation("异步方法")
    @GetMapping("async")
    public void testAsync() {
        long start = System.currentTimeMillis();
        log.info("异步方法开始");

        testService.asyncMethod();

        log.info("异步方法结束");
        long end = System.currentTimeMillis();
        log.info("总耗时：{} ms", end - start);
    }

    @ApiOperation("同步方法")
    @GetMapping("sync")
    public void testSync() {
        long start = System.currentTimeMillis();
        log.info("同步方法开始");

        testService.syncMethod();

        log.info("同步方法结束");
        long end = System.currentTimeMillis();
        log.info("总耗时：{} ms", end - start);
    }

    @ApiOperation("异步方法包含返回值")
    @GetMapping("async2")
    public void testAsync2() throws ExecutionException, InterruptedException {
        long start = System.currentTimeMillis();
        log.info("异步方法开始");

        Future<String> stringFuture = testService.asyncMethod2();
        //获取异步调用返回值
        String result = stringFuture.get();


        log.info("异步方法结束，返回值{}",result);
        long end = System.currentTimeMillis();
        log.info("总耗时：{} ms", end - start);
    }
}

```

> 结果：同步方法：执行2s，因为默认程序执行是从上到下的，必须等。异步方法：执行2ms，内部自己开启一个线程，继续往下执行，所以费时少。
>