# SpringBoot-Use-AOP-Customer-Log

# 1、Need Config

- LogAspect,定义日志切面类。

```java
@Aspect
@Component
public class LogAspect {
    @Resource
    private SysLogService sysLogService;

    @Pointcut("@annotation(com.wnx.mall.tiny.anno.Log)")
    public void pointcut(){}

    @Around("pointcut()")
    public Object around(ProceedingJoinPoint point){
        Object result = null;
        long beginTime = System.currentTimeMillis();

        try {
            //执行方法
             result = point.proceed();
        } catch (Throwable throwable) {
            throwable.printStackTrace();
        }
        
        //执行时常
        long time = System.currentTimeMillis() - beginTime;
        //保存日志
        saveLog(point,time);
        return result;

    }

    private void saveLog(ProceedingJoinPoint joinPoint,long time){
      MethodSignature methodSignature = (MethodSignature) joinPoint.getSignature();
        Method method = methodSignature.getMethod();
        SysLog sysLog = new SysLog();
        Log logAnnotation = method.getAnnotation(Log.class);
       
        if (logAnnotation !=null){
            //注解上的描述
            sysLog.setOperation(logAnnotation.value());
        }
        
        //请求的方法名
        String className = joinPoint.getTarget().getClass().getName();
        String methodName = methodSignature.getName();
        sysLog.setMethod(className + "."+methodName+"()");
       
        //请求的参数值
        Object[] args = joinPoint.getArgs();
      
        //请求的方法参数名称
        LocalVariableTableParameterNameDiscoverer u = new LocalVariableTableParameterNameDiscoverer();
        String[] parameterNames = u.getParameterNames(method);

        if (args!=null && parameterNames != null){
            String params = "";
            for (int i = 0; i < args.length; i++) {
                params += " " + parameterNames[i] + ":" + args[i];
            }
            sysLog.setParams(params);
        }

        //获取request
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();

        //设置IP地址
        sysLog.setIp(IPUtils.getIpAddr(request));
        
        //模拟一个用户
        sysLog.setUsername("wangnaixing");
        sysLog.setTime((int)time);
        sysLog.setCreateTime(new Date());
       
        //保存日志到数据库
        sysLogService.save(sysLog);


    }


}

```

- 自定义日志注解

```java
package com.wnx.mall.tiny.anno;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Log {
    String value() default  "";
}

```

- DataModel

```java
@Data
public class SysLog implements Serializable {
    @ApiModelProperty(value = "自增主键")
    private Long id;

    @ApiModelProperty(value = "用户名")
    private String username;

    @ApiModelProperty(value = "用户操作")
    private String operation;

    @ApiModelProperty(value = "响应时间")
    private String method;

    @ApiModelProperty(value = "请求方法")
    private String params;

    @ApiModelProperty(value = "IP地址")
    private String ip;

    @ApiModelProperty(value = "创建时间")
    private Date createTime;

    @ApiModelProperty(value = "花费时间")
    private Integer time;
  
}
```

# 2、Use @Log

> 观察@Log注解！

```java
public class PmsBrandController {
    
    @Autowired
    private PmsBrandService brandService;

    @ApiOperation("获取所有品牌列表")
    @Log("获取所有品牌列表")
    @RequestMapping(value = "listAll", method = RequestMethod.GET)
    @ResponseBody
    public CommonResult<List<PmsBrand>> getBrandList() {
        
        return CommonResult.success(brandService.listAllBrand());
        
    }

    @ApiOperation("添加品牌")
    @RequestMapping(value = "/create", method = RequestMethod.POST)
    @ResponseBody
    public CommonResult createBrand(@RequestBody PmsBrand pmsBrand) {
        CommonResult commonResult;
        int count = brandService.createBrand(pmsBrand);
        if (count == 1) {
            commonResult = CommonResult.success(pmsBrand);
            LOGGER.debug("createBrand success:{}", pmsBrand);
        } else {
            commonResult = CommonResult.failed("操作失败");
            LOGGER.debug("createBrand failed:{}", pmsBrand);
        }
        return commonResult;
    }

    @ApiOperation("更新指定id品牌信息")
    @Log("更新指定id品牌信息")
    @RequestMapping(value = "/update/{id}", method = RequestMethod.POST)
    @ResponseBody
    public CommonResult updateBrand(@PathVariable("id") Long id, @RequestBody PmsBrand pmsBrandDto, BindingResult result) {
        CommonResult commonResult;
        int count = brandService.updateBrand(id, pmsBrandDto);
        if (count == 1) {
            commonResult = CommonResult.success(pmsBrandDto);
            LOGGER.debug("updateBrand success:{}", pmsBrandDto);
        } else {
            commonResult = CommonResult.failed("操作失败");
            LOGGER.debug("updateBrand failed:{}", pmsBrandDto);
        }
        return commonResult;
    }

    @ApiOperation("删除指定id的品牌")
    @RequestMapping(value = "/delete/{id}", method = RequestMethod.GET)
    @ResponseBody
    @Log("删除指定id的品牌")
    public CommonResult deleteBrand(@PathVariable("id") Long id) {
        int count = brandService.deleteBrand(id);
        if (count == 1) {
            LOGGER.debug("deleteBrand success :id={}", id);
            return CommonResult.success(null);
        } else {
            LOGGER.debug("deleteBrand failed :id={}", id);
            return CommonResult.failed("操作失败");
        }
    }

    @ApiOperation("分页查询品牌列表")
    @Log("分页查询品牌列表")
    @RequestMapping(value = "/list", method = RequestMethod.GET)
    @ResponseBody
    public CommonResult<CommonPage<PmsBrand>> listBrand(@RequestParam(value = "pageNum", defaultValue = "1")
                                                        @ApiParam("页码") Integer pageNum,
                                                        @RequestParam(value = "pageSize", defaultValue = "3")
                                                        @ApiParam("每页数量") Integer pageSize) {
        List<PmsBrand> brandList = brandService.listBrand(pageNum, pageSize);
        
        return CommonResult.success(CommonPage.restPage(brandList));
    }

    @ApiOperation("获取指定id的品牌详情")
    @RequestMapping(value = "/{id}", method = RequestMethod.GET)
    @ResponseBody
    @Log("获取指定id的品牌详情")
    public CommonResult<PmsBrand> brand(@PathVariable("id") Long id) {
        return CommonResult.success(brandService.getBrand(id));
    }
}
```

# 3、Result



![image-20220106171750944](https://s2.loli.net/2022/01/06/zQZtLalyepcIu9V.png)