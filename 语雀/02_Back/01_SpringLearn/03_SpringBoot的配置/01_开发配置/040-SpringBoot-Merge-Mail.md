# SpringBoot-Merge-Mail

# 1、Need Config

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root
  mail:
    host: smtp.163.com
    username: gxuwz0722@163.com # 你的邮件账号
    password: SKDJQPWUJOJYPYHT # 授权客户端发邮件的授权码
    properties:
      mail:
        smtp:
          auth: true
          starttls:
            enable: true
            required: true

mybatis:
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**/mapper/*.xml

```

# 2、Your Business

> 一封邮件分为四大类：简单的纯文字邮件，HTML的邮件，带附件的邮件，带图片的邮件，模板式的邮件（验证码邮件）

```java
@Api(tags = "EmailController",description = "发送邮件测试")
@RestController
public class EmailController {
   
    @Value("${spring.mail.username}")
    private String from;
   
    @Resource
    private JavaMailSender sender;
    
    @Resource
    private TemplateEngine templateEngine;

    @GetMapping("/sendSimpleEmail")
    public CommonResult sendSimpleEmail(){
        //1.创建 SimpleMailMessage
        SimpleMailMessage message = new SimpleMailMessage();
        
        //设置发件人
        message.setFrom(from);
     	//设置接收人
        message.setTo("827376239@qq.com");
        //设置主题
        message.setSubject("一封简单的邮件");
        //设置正文
        message.setText("使用SpringBoot发送简单邮件。。。。");
        
        //2.执行发送
        sender.send(message);
       
        return CommonResult.success("");
    }

    @GetMapping("/sendHtmlEmail")
    public CommonResult sendHtmlEmail() throws MessagingException {
        //1.创建 MimeMessage
        MimeMessage message = sender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(message, true);
        
        //设置发件人
        helper.setFrom(from);
        //设置接收人
        helper.setTo("827376239@qq.com");
        //设置主题
        helper.setSubject("一封简单HTML内容的邮件");
        
        //设置带HTML格式的正文
        StringBuffer sb = new StringBuffer("<p style='color:#6db33f'>使用Spring Boot发送HTML格式邮件。</p>");
        helper.setText(sb.toString(),true);
        
       	//2.执行发送
        sender.send(message);
        
        return CommonResult.success("");
    }

    @GetMapping("/sendAttachmentsMail")
    public CommonResult sendAttachmentsMail() throws MessagingException {
        //1.创建 MimeMessage
        MimeMessage message = sender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(message,true);
      
        //设置发件人
        helper.setFrom(from);
        //设置接收人
        helper.setTo("827376239@qq.com");
        //设置主题
        helper.setSubject("一封有附件的邮件");
        //设置正文
        helper.setText("详情参照附件内容");
        
        //2.传入附件
        FileSystemResource file =
                new FileSystemResource(new File("src/main/resources/static/file/18软件6班-王乃醒-基于SpringBoot的高校就业信息平台的设计与实现-初稿.docx"));
        
        helper.addAttachment("毕业论文.docx",file);
        
        //3.执行发送
        sender.send(message);
        
        return CommonResult.success("");
    }
    @GetMapping("/sendInlineMail")
    public CommonResult sendInlineMail() throws MessagingException {
        //1.创建 MimeMessage
        MimeMessage message = sender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(message, true);
       
        //设置发件人
        helper.setFrom(from);
        //设置接收人
        helper.setTo("827376239@qq.com");
        //设置主题
        helper.setSubject("一封带有静态资源的邮件");
        //设置正文
        helper.setText("<html><body>博客图：<img src='cid:img'/></body></html>",true);
        
        //2.传入图片附件
        FileSystemResource file = new FileSystemResource(new File("src/main/resources/static/img/img.png"));
        helper.addInline("img",file);
        
        sender.send(message);
        
        return CommonResult.success("");
    }
    @GetMapping("sendTemplateEmail")
    public CommonResult sendTemplateEmail(String code) throws MessagingException {
         //1.创建 MimeMessage
        MimeMessage message = sender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(message,true);
        //设置发件人
        helper.setFrom(from);
        //设置接收人
        helper.setTo("827376239@qq.com");
        //设置主题
        helper.setSubject("一份模板邮件");
       
        //2.处理邮件模板
        Context context = new Context();
        
        context.setVariable("code", code);
        String template = templateEngine.process("emailTemplate", context);
        
        //3.将模板设置为正文
        helper.setText(template, true);
        
        //4.执行发送
        sender.send(message);
        
        helper.setText("详情参照附件内容");
        
        return CommonResult.success("");
    }
}


```

# 3、Location

- 附件和静态资源位置

![image-20220107175624184](https://s2.loli.net/2022/01/07/fTB2DwOVmeQGRNt.png)

- template模板位置

![image-20220107175647249](https://s2.loli.net/2022/01/07/J5NdaHlPypLFbDM.png)