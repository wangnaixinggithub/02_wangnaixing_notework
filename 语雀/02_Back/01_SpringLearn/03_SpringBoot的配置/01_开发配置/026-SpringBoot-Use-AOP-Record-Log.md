# SpringBoot-Use-AOP-Record-Log

- 数据存入webLog中，可以进一步存到数据库。

```java
@Data
public class WebLog {
    @ApiModelProperty("操作描述")
    private String description;
    @ApiModelProperty("操作用户")
    private String username;
    @ApiModelProperty("操作时间")
    private Long startTime;
    @ApiModelProperty("消耗时间")
    private Integer spendTime;
    @ApiModelProperty("根路径")
    private String basePath;
    @ApiModelProperty("URI")
    private String uri;
    @ApiModelProperty("URL")
    private String url;
    @ApiModelProperty("请求类型")
    private String method;
    @ApiModelProperty("IP地址")
    private String ip;
    @ApiModelProperty("请求参数")
    private Object parameter;
    @ApiModelProperty("请求返回的结果")
    private Object result;
}
```

```java
@Aspect
@Component
@Order(1)
public class WebLogAspect {
    private static final Logger LOGGER = LoggerFactory.getLogger(WebLogAspect.class);

    @Pointcut("execution(public * com.wnx.mall.tiny.controller.*.*(..))")
    public void  webLog(){}

    @Before("webLog()")
    public void deBefore(JoinPoint joinPoint){}
    @AfterReturning(value = "webLog()",returning = "ret")
    public void doAfterReturning(Object ret){}
    @Around("webLog()")
    public Object doAround(ProceedingJoinPoint joinPoint) throws Throwable {
        long startTime = System.currentTimeMillis();
  
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
     
        WebLog webLog = new WebLog();
        
        Object result = joinPoint.proceed();
        
        Signature signature = joinPoint.getSignature();
        MethodSignature methodSignature = (MethodSignature) signature;
        Method method = methodSignature.getMethod();
       
        if (method.isAnnotationPresent(ApiOperation.class)){
            ApiOperation apiOperation = method.getAnnotation(ApiOperation.class);
            webLog.setDescription(apiOperation.value());
        }
        
        long endTime = System.currentTimeMillis();
        
        String urlStr = request.getRequestURL().toString();
        webLog.setBasePath(StrUtil.removeSuffix(urlStr, URLUtil.url(urlStr).getPath()));
        
        webLog.setIp(request.getRemoteUser());
        
        webLog.setMethod(request.getMethod());
        
        webLog.setParameter(getParameter(method,joinPoint.getArgs()));
        
        webLog.setResult(result);
        
        webLog.setSpendTime((int) (endTime - startTime));
        
        webLog.setStartTime(startTime);
        
        webLog.setUri(request.getRequestURI());
        
        webLog.setUrl(request.getRequestURL().toString());
        
        LOGGER.info("{}", JSONUtil.parse(webLog));
        
        return result;

    }

    /**
     * 根据方法和传入的参数获取请求参数
     */
    private Object getParameter(Method method, Object[] args) {
        List<Object> argList = new ArrayList<>();
        Parameter[] parameters = method.getParameters();
        for (int i = 0; i < parameters.length; i++) {
            
            //将RequestBody注解修饰的参数作为请求参数
            RequestBody requestBody = parameters[i].getAnnotation(RequestBody.class);
            if (requestBody != null) {
                argList.add(args[i]);
            }
            
            //将RequestParam注解修饰的参数作为请求参数
            RequestParam requestParam = parameters[i].getAnnotation(RequestParam.class);
            if (requestParam != null) {
                Map<String, Object> map = new HashMap<>();
                String key = parameters[i].getName();
                if (!StringUtils.isEmpty(requestParam.value())) {
                    key = requestParam.value();
                }
                map.put(key, args[i]);
                argList.add(map);
            }
        }
        
        if (argList.size() == 0) {
            
            return null;
            
        } else if (argList.size() == 1) {
            
            return argList.get(0);
            
        } else {
            
            return argList;
        }
    }
}


```


