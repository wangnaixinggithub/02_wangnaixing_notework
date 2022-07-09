# SpringMvc-Handle-Properties

假如现在要实现一个用于处理 `Content-Type` 为 `text/properties `媒体类型.

当我们在请求体中传输下面内容时,他能转化为Properties对象.

```properties
username:root
password:root
url:jdbc:mysql//localhost:3309/mall
```



```java
package com.wnx.mall.tiny.controller;

import com.wnx.mall.tiny.common.api.CommonResult;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.Properties;


@RestController
@Api(tags = "ConverterPropertiesController", description = "消息转换器接口")
public class ConverterPropertiesController {

    @ApiOperation("测试Properties对象接收")
    @GetMapping(value = "/test",consumes = "text/properties")
    public Properties test(@RequestBody Properties properties){
        System.out.println(properties);
        return properties;
    }

}

```

# 1、Need Config

> 创建`PropertiesHttpMessageConverter`，继承`AbstractGenericHttpMessageConverter`

```java
public class PropertiesHttpMessageConverter extends AbstractGenericHttpMessageConverter<Properties> {
    
    
    public PropertiesHttpMessageConverter() {
        //1-指定它能处理的媒体类型！
        super(new MediaType("text", "properties"));
    }
		
    //2-读入
    @Override
    protected Properties readInternal(Class<? extends Properties> clazz, HttpInputMessage inputMessage) throws IOException, HttpMessageNotReadableException {
        Properties properties = new Properties();
         Charset charset = null;
        
        //1.获取请求头
        HttpHeaders headers = inputMessage.getHeaders();
        
        //2.获取 content-type
        MediaType contentType = headers.getContentType();
        
        //3.获取编码
        if (contentType != null) {charset = contentType.getCharset();}
        charset = charset == null ? Charset.forName("UTF-8") : charset;

        //4.获取请求体
        InputStream body = inputMessage.getBody();
       
        //5.输入流阅读者
        InputStreamReader inputStreamReader = new InputStreamReader(body, charset);
        
        properties.load(inputStreamReader);
        
        return properties;
    }
   //3-写出
   @Override
    protected void writeInternal(Properties properties, Type type, HttpOutputMessage outputMessage) throws IOException, HttpMessageNotWritableException {
        Charset charset = null;
        
        //1.获取请求头
        HttpHeaders headers = outputMessage.getHeaders();
        
        //2.获取 content-type
        MediaType contentType = headers.getContentType();
      
        //3.获取编码
        if (contentType != null) {charset = contentType.getCharset();}
        charset = charset == null ? Charset.forName("UTF-8") : charset;

        // 获取请求体
        OutputStream body = outputMessage.getBody();
        OutputStreamWriter outputStreamWriter = new OutputStreamWriter(body, charset);

        properties.store(outputStreamWriter, "Serialized by PropertiesHttpMessageConverter#writeInternal");
    }
}
```

```java
package com.wnx.mall.tiny.config;

import com.wnx.mall.tiny.converter.PropertiesHttpMessageConverter;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.List;

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        converters.add(0, new PropertiesHttpMessageConverter()); //排在第一个！
    }
}

```

