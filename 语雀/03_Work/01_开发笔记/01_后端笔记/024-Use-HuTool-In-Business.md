# Use-HuTool-In-Business

# 1、类型转换

```java
    @ApiOperation("Convert使用：类型转换工具类")
    @GetMapping(value = "/covert")
    public CommonResult covert() {
        //转换成字符串
        int a = 1;
        String aStr = Convert.toStr(a);
      
        //转换为指定类型数组
        String[] b = {"1","2","3","4"};
        Integer[] bArr = Convert.toIntArray(b);
        
        //转换成日期对象
        String dateStr = "2017-07-22";
        Date date = Convert.toDate(dateStr);
       
        //转换成列表
        String[] strArr = {"a","b","c","d"};
        List<String> strList = Convert.toList(String.class, strArr);
        return CommonResult.success(null,"操作成功");
    }
```



# 2、日期时间

```java
     @ApiOperation("DateUtil使用：日期时间工具")
    @GetMapping("/dateUtil")
    public CommonResult dateUtil(){
        //Date、long、Calendar之间的转换
         //当前时间
          Date date = DateUtil.date();
        //Calendar转Date
         date = DateUtil.date(Calendar.getInstance());
        //时间戳转Date
        date = DateUtil.date(System.currentTimeMillis());
        //自动识别格式转换
        String dateStr = "2012-12-21";
        date = DateUtil.parse(dateStr);
        //自定义格式转换
        DateUtil.parse(dateStr,"yyyy-MM-dd");
        //格式化输出日期
        String format = DateUtil.format(date,"yyyy-MM-dd");
        //获取年的部分
        int year = DateUtil.year(date);
        //获取月份，从0开始。
        int month = DateUtil.month(date);
        //获取某天的开始时间 2012-12-21 23:59:59
        Date beginOfDay = DateUtil.beginOfDay(date);
        //获取某天的结束时间 2012-12-21 00:00:00
        Date endOfDay = DateUtil.endOfDay(date);
        //计算偏移后的日期
        Date newDate = DateUtil.offset(date, DateField.DAY_OF_MONTH, 2);
        //计算日期中间的偏移量
        long betweenDay = DateUtil.between(date, newDate, DateUnit.DAY);//2天
        return CommonResult.success(null,"操作成功！");
    }
```



# 3、字符串

```java
 @ApiOperation("StrUtil使用：字符串工具")
    @GetMapping("/strUtil")
    public CommonResult strUtil(){
        //判断是否为空字符串？(null或者’‘都认定为空字符串)
        String str = null;
        StrUtil.isNotBlank(str);
        StrUtil.isBlank(str);
        //去除字符串的前后缀
        StrUtil.removeSuffix("a.jpg",".jpg");
        StrUtil.removePrefix("a.jpg","a.") ;
        //格式化字符串
        String template = "这只是一个占位符：{}";
        String str2 = StrUtil.format(template,"我是占位符");
        LOGGER.info("/strUtil format:{}",str2);
        return CommonResult.success(null,"操作成功！");
    }
```



# 4、读取Resource下文件

```java
 @ApiOperation("ClassPath单一资源访问类：在classpath下查找文件")
    @GetMapping("/classPath")
    public CommonResult classPath() throws IOException {
        //获取定义在src/main/resources文件夹中的配置文件
        ClassPathResource resource = new ClassPathResource("generator.properties");
        Properties prop = new Properties();
        prop.load(resource.getStream());
        LOGGER.info("/classpath:{}",prop);
        return CommonResult.success(null,"操作成功！");
    }
```



# 5、反射工具类

```java
 @ApiOperation("ReflectUtil使用：JAVA反射工具类")
    @GetMapping("/reflectUtil")
    public CommonResult reflectUtil(){
        //获取某个类的所有方法
        Method[] methods = ReflectUtil.getMethods(PmsBrand.class);
        //获取某个类的指定方法
        Method method = ReflectUtil.getMethod(PmsBrand.class, "getId");
        //通过反射来创建对象
        PmsBrand pmsBrand = ReflectUtil.newInstance(PmsBrand.class);
        //反射执行对象的方法
        ReflectUtil.invoke(pmsBrand,"setId",1);
        return CommonResult.success(null,"操作成功！");
    }
```



# 6、Bean相关

```java
 @ApiOperation("BeanUtil使用，JavaBean的工具类")
    @GetMapping("/beanUtil")
    public CommonResult beanUtil(){
        PmsBrand brand = new PmsBrand();
        brand.setId(1L);
        brand.setName("小米");
        brand.setShowStatus(0);
        //Bean转Map
        Map<String, Object> mapBread = BeanUtil.beanToMap(brand);
        LOGGER.info("beanUtil map to bean:{}",mapBread);
        //Bean属性拷贝
        PmsBrand copyBrand = new PmsBrand();
        BeanUtils.copyProperties(brand,copyBrand);
        LOGGER.info("beanUtil copy properties:{}",copyBrand);
        return CommonResult.success(null,"操作成功！");
    }
```



# 7、Map相关

```java
  @GetMapping("/mapUtil")
    public CommonResult mapUtil(){
        //将多个键值对加入到Map中
        Map<Object, Object> map = MapUtil.of(new String[][]{
                {"key1", "value1"},
                {"key2", "value2"},
                {"key3", "value3"},
        });

        //判断Map是否为空？
        MapUtil.isEmpty(map);
        MapUtil.isNotEmpty(map);
        return CommonResult.success(null,"操作成功！");
    }
```



# 8、注解工具

```java
 @ApiOperation("AnnotationUtil使用：注解工具类")
    @GetMapping("/annotationUtil")
    public CommonResult annotationUtil(){
        //获取指定类、方法、字段、构造器上的注解列表
        Annotation[] annotations = AnnotationUtil.getAnnotations(HutoolController.class, false);
        LOGGER.info("annotationUtil annotations:{} ",annotations[1]);
        //获取指定类型的注解
        Api api = AnnotationUtil.getAnnotation(HutoolController.class, Api.class);
        LOGGER.info("annotationUtil api value:{}",api.description());
        //获取指定类型注解的值
        Object annotationValue = AnnotationUtil.getAnnotationValue(HutoolController.class, RequestMapping.class);
        LOGGER.info("annotationUtil annotationValue:{} ",annotationValue);
        return CommonResult.success(null,"操作成功！");
    }
```



# 9、JSON解析

```java
    @ApiOperation("JSONUtil使用:JSON解析工具类")
    @GetMapping("/jsonUtil")
    public CommonResult jsonUtil(){
        PmsBrand brand = new PmsBrand();
        brand.setId(1L);
        brand.setName("小米");
        brand.setShowStatus(1);
        //对象转换为JSON字符串
        String jsonStr = JSONUtil.parse(brand).toString();
        LOGGER.info("jsonUtil parse:{}",jsonStr);
        //JSON字符串转化为对象
        PmsBrand pmsBrand = JSONUtil.toBean(jsonStr, PmsBrand.class);
        LOGGER.info("jsonUtil toBean:{}",pmsBrand);
        List<PmsBrand> brandList = new ArrayList<>();
        brandList.add(pmsBrand);
        String jsonListStr = JSONUtil.parse(brandList).toString();
        //JSON字符串转换为列表
        List<PmsBrand> pmsBrandList = JSONUtil.toList(new JSONArray(jsonListStr), PmsBrand.class);
        LOGGER.info("jsonUtil toList:{}",pmsBrandList);
        return CommonResult.success(null,"操作成功！");
    }
```



# 10、验证码

```java
 @ApiOperation("CaptchaUtil使用：图形验证码")
    @GetMapping("/captchaUtil")
    public void captchaUtil(HttpServletRequest request, HttpServletResponse response)  {
        //生成验证码图片
        CircleCaptcha lineCaptcha = CaptchaUtil.createCircleCaptcha(200, 100);
        try {
            request.getSession().setAttribute("CAPTCHA_KEY", lineCaptcha.getCode());
            response.setContentType("image/png");//告诉浏览器输出内容为图片
            response.setHeader("Pragma", "No-cache");//禁止浏览器缓存
            response.setHeader("Cache-Control", "no-cache");
            response.setDateHeader("Expire", 0);
            lineCaptcha.write(response.getOutputStream());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```



# 11、字段校验

```java
    @ApiOperation("Validator使用:字段验证器")
    @GetMapping("/validator")
    public CommonResult validator(){
        //判断是否为邮箱地址
        boolean result = Validator.isEmail("wangnaixing@qq.com");
        LOGGER.info("Validator isEmail:{}",result);
        //判断是否为手机号码
         result = Validator.isMobile("18154622909");
         LOGGER.info("Validator isMobile:{}",result);
        //判断是否为IPV4地址
        result = Validator.isIpv4("192.168.3.101");
        LOGGER.info("Validator isIpv4:{}",result);
        //判断是否为汉字
        result = Validator.isChinese("你好");
        LOGGER.info("Validator isChinese:{}",result);
        //判断是否为身份证号（18位中国）
        result =  Validator.isCitizenId("123456");
        LOGGER.info("Validator isCitizenId:{}",result);
        //判断是否为URL
        result  = Validator.isUrl("http://www.baidu.com");
        LOGGER.info("Validator isUrl:{}",result);
        //判断是否为生日
        result = Validator.isBirthday("2020-12-14");
        LOGGER.info("Validator isBirthday:{}",result);
        return CommonResult.success(null,"操作成功！");
    }
```



# 12、密码加密

```java
    @ApiOperation("DigestUtil使用：摘要算法工具类")
    @GetMapping("/digestUtil")
    public CommonResult digestUtil(){
        String password = "123456";
        //计算MD5摘要值，并转为16进制字符串
        String result = DigestUtil.md5Hex(password);
        LOGGER.info("digestUtil md5Hex:{}",result);
        //计算SHA-256摘要值，并转为16进制字符串
        result = DigestUtil.sha256Hex(password);
        LOGGER.info("digestUtil sha256Hex:{}",result);
        //生成Bcrypt加密后的密文，并检验。
        String hashPwd = DigestUtil.bcrypt(password);
        boolean check = DigestUtil.bcryptCheck(password, hashPwd);
        LOGGER.info("digestUtil bcryptCheck:{}",check);
        return CommonResult.success(null,"操作成功！");
    }

```



# 13、HTTP请求发送

```java
 @ApiOperation("HttpUtil使用：Http请求工具类")
    @GetMapping("/httpUtil")
    public CommonResult httpUtil(){
        String response = HttpUtil.get("http://localhost:8080/hutool/covert");
        LOGGER.info("HttpUtil get:{}", response);
        return CommonResult.success(null, "操作成功");
    }
```

