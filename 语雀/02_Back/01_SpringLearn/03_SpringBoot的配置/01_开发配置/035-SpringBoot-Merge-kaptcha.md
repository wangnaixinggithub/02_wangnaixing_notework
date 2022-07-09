# SpringBoot-Merge-kaptcha

# 1、Need Config

```xml
<!-- 验证码 -->
<dependency>
    <groupId>com.github.penggle</groupId>
    <artifactId>kaptcha</artifactId>
    <version>2.3.2</version>
</dependency>
```

```java
package com.google.code.kaptcha;

public class Constants {
    /**
     * 描述: session key
     * 默认值: KAPTCHA_SESSION_KEY
     * 合法值:
     */
    public final static String KAPTCHA_SESSION_KEY = "KAPTCHA_SESSION_KEY";

    /**
     * 描述: session date
     * 默认值: KAPTCHA_SESSION_DATE
     * 合法值:
     */
    public final static String KAPTCHA_SESSION_DATE = "KAPTCHA_SESSION_DATE";

    /**
     * 描述:
     * 默认值:
     * 合法值:
     */
    public final static String KAPTCHA_SESSION_CONFIG_KEY = "kaptcha.session.key";

    /**
     * 描述:
     * 默认值:
     * 合法值:
     */
    public final static String KAPTCHA_SESSION_CONFIG_DATE = "kaptcha.session.date";

    /**
     * 描述: 图片边框
     * 默认值: yes
     * 合法值: yes/no
     */
    public final static String KAPTCHA_BORDER = "kaptcha.border";

    /**
     * 描述: 边框颜色
     * 默认值: black
     * 合法值: r,g,b (and optional alpha) 或者 white,black,blue.
     */
    public final static String KAPTCHA_BORDER_COLOR = "kaptcha.border.color";

    /**
     * 描述: 边框厚度
     * 默认值:
     * 合法值:
     */
    public final static String KAPTCHA_BORDER_THICKNESS = "kaptcha.border.thickness";

    /**
     * 描述: 干扰 颜色
     * 默认值: black
     * 合法值: r,g,b 或者 white,black,blue.
     */
    public final static String KAPTCHA_NOISE_COLOR = "kaptcha.noise.color";

    /**
     * 描述: 干扰实现类
     * 默认值: com.google.code.kaptcha.impl.DefaultNoise
     * 合法值:
     */
    public final static String KAPTCHA_NOISE_IMPL = "kaptcha.noise.impl";

    /**
     * 描述: 图片样式
     * 默认值: 水纹 com.google.code.kaptcha.impl.WaterRipple
     * 合法值: 水纹 com.google.code.kaptcha.impl.WaterRipple
     *        鱼眼 com.google.code.kaptcha.impl.FishEyeGimpy
     *        阴影 com.google.code.kaptcha.impl.ShadowGimpy
     */
    public final static String KAPTCHA_OBSCURIFICATOR_IMPL = "kaptcha.obscurificator.impl";

    /**
     * 描述: 图片实现类
     * 默认值: com.google.code.kaptcha.impl.DefaultKaptcha
     * 合法值:
     */
    public final static String KAPTCHA_PRODUCER_IMPL = "kaptcha.producer.impl";

    /**
     * 描述: 文本实现类
     * 默认值: com.google.code.kaptcha.text.impl.DefaultTextCreator
     * 合法值:
     */
    public final static String KAPTCHA_TEXTPRODUCER_IMPL = "kaptcha.textproducer.impl";

    /**
     * 描述: 文本集合，验证码值从此集合中获取
     * 默认值: abcde2345678gfynmnpwx
     * 合法值:
     */
    public final static String KAPTCHA_TEXTPRODUCER_CHAR_STRING = "kaptcha.textproducer.char.string";

    /**
     * 描述: 验证码长度
     * 默认值: 5
     * 合法值:
     */
    public final static String KAPTCHA_TEXTPRODUCER_CHAR_LENGTH = "kaptcha.textproducer.char.length";

    /**
     * 描述: 字体
     * 默认值: Arial, Courier
     * 合法值:
     */
    public final static String KAPTCHA_TEXTPRODUCER_FONT_NAMES = "kaptcha.textproducer.font.names";

    /**
     * 描述: 字体颜色，合法值：r,g,b 或者 white,black,blue.
     * 默认值: black
     * 合法值:
     */
    public final static String KAPTCHA_TEXTPRODUCER_FONT_COLOR = "kaptcha.textproducer.font.color";

    /**
     * 描述: 字体大小
     * 默认值: 40px
     * 合法值:
     */
    public final static String KAPTCHA_TEXTPRODUCER_FONT_SIZE = "kaptcha.textproducer.font.size";

    /**
     * 描述: 文字间隔
     * 默认值: 2
     * 合法值:
     */
    public final static String KAPTCHA_TEXTPRODUCER_CHAR_SPACE = "kaptcha.textproducer.char.space";

    /**
     * 描述: 文字渲染器
     * 默认值: com.google.code.kaptcha.text.impl.DefaultWordRenderer
     * 合法值:
     */
    public final static String KAPTCHA_WORDRENDERER_IMPL = "kaptcha.word.impl";

    /**
     * 描述: 背景实现类
     * 默认值: com.google.code.kaptcha.impl.DefaultBackground
     * 合法值:
     */
    public final static String KAPTCHA_BACKGROUND_IMPL = "kaptcha.background.impl";

    /**
     * 描述: 背景颜色渐变，开始颜色
     * 默认值: light grey
     * 合法值:
     */
    public static final String KAPTCHA_BACKGROUND_CLR_FROM = "kaptcha.background.clear.from";

    /**
     * 描述: 背景颜色渐变， 结束颜色
     * 默认值: white
     * 合法值:
     */
    public static final String KAPTCHA_BACKGROUND_CLR_TO = "kaptcha.background.clear.to";

    /**
     * 描述: 图片宽
     * 默认值: 200
     * 合法值:
     */
    public static final String KAPTCHA_IMAGE_WIDTH = "kaptcha.image.width";

    /**
     * 描述: 图片高
     * 默认值: 50
     * 合法值:
     */
    public static final String KAPTCHA_IMAGE_HEIGHT = "kaptcha.image.height";
}

```

```java
@Component
public class KaptchaConfig {

    @Bean
    public DefaultKaptcha getDefaultKaptcha() {
        DefaultKaptcha kaptcha = new DefaultKaptcha();
        Properties properties = new Properties();

        /**
         * kaptcha.border 是否有图片边框
         */
        properties.setProperty(Constants.KAPTCHA_BORDER, "no");

        /**
         * kaptcha.border.color 边框颜色
         */
        properties.setProperty(Constants.KAPTCHA_BORDER_COLOR, "105,179,90");

        /**
         * kaptcha.textproducer.font.color 字体颜色
         */
        properties.setProperty(Constants.KAPTCHA_TEXTPRODUCER_FONT_COLOR, "black");

        /**
         * kaptcha.image.width 图片宽
         */
        properties.setProperty(Constants.KAPTCHA_IMAGE_WIDTH, "135");

        /**
         * kaptcha.image.height 图片高
         */
        properties.setProperty(Constants.KAPTCHA_IMAGE_HEIGHT, "30");

        /**
         * kaptcha.textproducer.font.size 字体大小
         */
        properties.setProperty(Constants.KAPTCHA_TEXTPRODUCER_FONT_SIZE, "24");

        /**
         * kaptcha.session.key session key值
         */
        properties.setProperty(Constants.KAPTCHA_SESSION_KEY, "kaptchaCode");

        /**
         * kaptcha.textproducer.char.length 验证码长度
         */
        properties.setProperty(Constants.KAPTCHA_TEXTPRODUCER_CHAR_LENGTH, "5");

        /**
         * kaptcha.textproducer.char.string 使用那些字符生成验证码
         */
        properties.setProperty(Constants.KAPTCHA_TEXTPRODUCER_CHAR_STRING, "ABCDEFGHIJKMLNOPQRSTUVWXYZabcdefghijkmlopqrstuvwxyz0123456789");

        /**
         * kaptcha.textproducer.font.names 使用哪些字体
         */
        properties.setProperty(Constants.KAPTCHA_TEXTPRODUCER_FONT_NAMES, "Arial");

        /**
         * kaptcha.noise.color 干扰线颜色
         */
        properties.setProperty(Constants.KAPTCHA_NOISE_COLOR, "gray");

        /**
         * kaptcha.obscurificator.impl 图片样式阴影
         * WaterRipple: 为图像添加水波纹效果
         * ShadowGimpy: 为图像上的文字添加阴影和两个噪点
         */
        properties.setProperty(Constants.KAPTCHA_OBSCURIFICATOR_IMPL, "com.google.code.kaptcha.impl.ShadowGimpy");

        Config config = new Config(properties);
        kaptcha.setConfig(config);
        return kaptcha;

    }

}
```

# 2、Your Business

- KaptchaServlet,显示验证码的接口。

```java
public class KaptchaServlet extends HttpServlet {
    
    @Autowired
    private DefaultKaptcha defaultKaptcha;

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        try {
            ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
           
            // step 生产验证码字符串并保存到session中
            String code = defaultKaptcha.createText();
            req.getSession().setAttribute(Constants.KAPTCHA_SESSION_KEY, code);
            System.out.println("产生的Code:" + code);
            
            //把图片的流给outputStream
            BufferedImage image = defaultKaptcha.createImage(code);
            ImageIO.write(image, "jpg", outputStream);
            
            
            
            //向浏览器返回验证码图片
            byte[] bytes = outputStream.toByteArray();
            resp.setHeader("Cache-Control", "no-store");
            resp.setHeader("Pragma", "no-cache");
            resp.setDateHeader("Expires", 0);
            resp.setContentType("image/jpeg");
            ServletOutputStream servletOutputStream = resp.getOutputStream();
            servletOutputStream.write(bytes);
            servletOutputStream.flush();
            servletOutputStream.close();
            
            
        } catch (Exception e) {
            // step 出错的话将跳转到错误页面
            resp.sendError(HttpServletResponse.SC_NOT_FOUND);
        }
    
    }
}

```

- 校验验证码的过滤器，CheckFilter.

```java

public class CheckFilter implements Filter {

@Override
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
     
       	// 1、获取参数的中验证码
        String param = request.getParameter("code");

        // 2、session中的验证码
        HttpServletRequest httpServletRequest = (HttpServletRequest) request;
        String sessionValue = (String) httpServletRequest.getSession().getAttribute(Constants.KAPTCHA_SESSION_KEY);

        // 3、校验验证码 GIVE Some Handler!
        if (    StringUtils.isEmpty(param) ||
                StringUtils.isEmpty(sessionValue) ||
                sessionValue.equals(param)) {

                chain.doFilter(request, response);
                return;
        }

  		
        HttpServletResponse httpServletResponse = (HttpServletResponse) response;
        httpServletResponse.sendError(HttpServletResponse.SC_NOT_FOUND, "验证码错误");

   }
}
```

- 注册进SpringMvc

```java
@Configuration
public class WebServletConfig {
    
    	
    //1.获取Servlet
    @Bean
    public KaptchaServlet getKaptchaServlet() { return new KaptchaServlet(); }
    
    
    //2.通过Filter 校验验证码
    @Bean
    public CheckFilter getCheckFilter() { return new CheckFilter(); }


	
    @Bean
    public ServletRegistrationBean servletRegistrationBean() {
        ServletRegistrationBean servlet = new ServletRegistrationBean();
        servlet.setServlet(getKaptchaServlet());
        servlet.addUrlMappings("/user/code");
        return servlet;
    }

    @Bean
    public FilterRegistrationBean filterRegistrationBean() {
        FilterRegistrationBean filter = new FilterRegistrationBean();
        filter.setFilter(getCheckFilter());
        filter.addUrlPatterns("/user/login");
        return filter;
    }
}

```

- 配置登录页，以及处理登录请求。 登录接口。

```java


@Controller
@RequestMapping("/user")
public class UserController {

    @GetMapping("/page")
    public String page(){
        return"login";
    }


    @PostMapping("/login")
    @ResponseBody
    public Map<String, String> login(LoginEntity login, HttpServletRequest request) {
        String attribute = (String) request.getSession().getAttribute(Constants.KAPTCHA_SESSION_KEY);
        System.out.println(attribute);
        System.out.println(login);

        return null;
    }
}

```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
<div>
    <form action="/user/login" method="post">
        <div>
            <label>账号</label><input name="username" value=""/>
        </div>
        <div>
            <label>密码</label><input name="password" value=""/>
        </div>
        <div>
            <input name="code" value=""><img src="/user/code" alt="">
        </div>
        <div>
            <button type="submit">登录</button>
            <button type="reset">取消</button>
        </div>
    </form>
</div>
</body>
</html>
```

![](D:\my_notebook\Springboot之验证码配置\QQ截图20211202100152.png)