# SpringMVC-XmlConfig-MessageConverter

HttpMessageConverter接口：（有基于jackson,gson,fastJson的三个实现类）

> 负责把返回的对象转为JSON，把前端的JSON转为对象 ,此过程叫:序列化和反序列化的过程

## 1、MapperingJackson2HttpMessageConverter

SpringMVC已经提供基于jackson的这个实现类，开发者只需要把jackson依赖导入就可实现序列化和反序列化的事情。

- 加入依赖

```xml
   <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.13.2.2</version>
        </dependency>
```

- 得到JSON对象或者JSON数组

```java
@Controller
@RequestMapping("/book")
public class JsonController {

    @GetMapping("/getById")
    @ResponseBody
    public Book getObject(){
     return    Book.builder().id(1).name("JAVA精通大师").author("王乃醒").published(LocalDate.now()).build();
    }
    
    @ResponseBody
    @GetMapping("findAll")
    public List<Book> getAll(){
        ArrayList<Book> books = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
           books.add( Book.builder().id(1).name("JAVA精通大师"+i).author("王乃醒"+i).published(LocalDate.now()).build()) ;
        }
        return books;
    }
}

```

```json
{"id":1,"name":"JAVA精通大师","author":"王乃醒","published":"2022-03-06"}
```

```json
[{"id":1,"name":"JAVA精通大师0","author":"王乃醒0","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师1","author":"王乃醒1","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师2","author":"王乃醒2","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师3","author":"王乃醒3","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师4","author":"王乃醒4","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师5","author":"王乃醒5","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师6","author":"王乃醒6","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师7","author":"王乃醒7","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师8","author":"王乃醒8","published":"2022-03-06"},{"id":1,"name":"JAVA精通大师9","author":"王乃醒9","published":"2022-03-06"}]
```

## 2、日期字段需要额外处理

### 2.1、Date字段转化处理

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Integer id;
    private String name;
    private String author;
    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")
    private Date published;
}

```

### 2.2、LocalDate And LocalDateTime

> Jaskson本来就LocalDateTime LocalDate 对于Java8的新日期APi并没有提供想要的序列化器和解析器，后优化跟上节奏才推出了，需要额外导包。

```xml
     <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.13.2.2</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.datatype</groupId>
            <artifactId>jackson-datatype-jsr310</artifactId>
            <version>2.13.2</version>
        </dependency>
```

- LocalDate

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Integer id;
    private String name;
    private String author;
    @JsonDeserialize(using = LocalDateDeserializer.class)
    @JsonSerialize(using = LocalDateSerializer.class)
    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")
    private LocalDate published;
}

```

- LocalDateTime

```java

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private Integer id;
    private String name;
    private String author;
    @JsonDeserialize(using = LocalDateTimeDeserializer.class)
    @JsonSerialize(using = LocalDateTimeSerializer.class)
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone = "Asia/Shanghai")
    private LocalDateTime published;
}
```

## 3、日期全局配置

### 3.1、Date类型的全局配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/mvc
	http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">


    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="defaultEncoding" value="UTF-8"/>
        <property name="maxUploadSize" value="10485760"/>
        <property name="maxUploadSizePerFile" value="10485760"/>
        <property name="maxInMemorySize" value="4096"/>
        <property name="uploadTempDir" value="/upload/temp"/>
    </bean>

    <context:component-scan base-package="com.wnx.springmvc" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
     <!--
          1。配置自定义消息转换器，加入对Date类型的处理
      -->
    <bean id="httpMessageConverter" class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
        <property name="objectMapper">
            <bean class="com.fasterxml.jackson.databind.ObjectMapper">
                <!--1.日期格式化和时区格式化-->
                <property name="dateFormat">
                    <bean class="java.text.SimpleDateFormat">
                        <constructor-arg name="pattern" value="yyyy-MM-dd HH:mm:ss"/>
                    </bean>
                </property>
                <property name="timeZone" value="Asia/Shanghai"/>
            </bean>
        </property>
        <!--2.设置中文编码格式-->
        <property name="supportedMediaTypes">
            <list>
                <value>application/json</value>
            </list>
        </property>
    </bean>
    <!--
        2.注入到消息转换器集合中
    -->
    <mvc:annotation-driven>
        <mvc:message-converters>
            <ref bean="httpMessageConverter"/>
        </mvc:message-converters>
    </mvc:annotation-driven>

    <bean class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <ref bean="dateConverter"/>
            </set>
        </property>
    </bean>
    <bean id="templateResolver" class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
        <property name="prefix" value="/WEB-INF/"/>
        <property name="suffix" value=".html"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="order" value="1"/>
        <property name="templateMode" value="HTML"/>
        <property name="cacheable" value="false"/>
    </bean>
    <bean id="templateEngine" class="org.thymeleaf.spring5.SpringTemplateEngine">
        <property name="templateResolver" ref="templateResolver"/>
    </bean>
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="templateEngine" ref="templateEngine"/>
        <property name="characterEncoding" value="UTF-8"/>
    </bean>
    <mvc:resources mapping="/**" location="/"/>

  
    <bean id="validatorFactoryBean" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
        <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
    </bean>


</beans>
```

> 好处：不用在POJO重复定义    @JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")

### 3.2、Date、LocalDate、LocalDateTime的全局配置

> 我的思路是：继承ObjectMapper接口，做增强。配置Date日期格式化的属性。
>
> SimpleModule加入LocalDateDeserializer LocalDateSerializer LocalDateTimeDeserializer LocalDateTimeSerializer 这四个解析器。

- CustomObjectMapper

```java
@Component
public class CustomObjectMapper  extends ObjectMapper {
    public CustomObjectMapper() {
        //1.Date处理
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        this.setDateFormat(sdf);
        this.setTimeZone(TimeZone.getTimeZone(ZoneId.systemDefault()));
        this.setLocale(Locale.getDefault());

        SimpleModule simpleModule = new SimpleModule();

        //2.LocalDate LocalDateTime LocalTime 处理
        simpleModule.addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
        simpleModule.addSerializer(LocalDate.class, new LocalDateSerializer(DateTimeFormatter.ofPattern("yyyy-MM-dd")));
        simpleModule.addSerializer(LocalTime.class, new LocalTimeSerializer(DateTimeFormatter.ofPattern("HH:mm:ss")));
        simpleModule.addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));
        simpleModule.addDeserializer(LocalDate.class, new LocalDateDeserializer(DateTimeFormatter.ofPattern("yyyy-MM-dd")));
        simpleModule.addDeserializer(LocalTime.class, new LocalTimeDeserializer(DateTimeFormatter.ofPattern("HH:mm:ss")));

        //3.把Long类型序列化为String类型
        simpleModule.addSerializer(Long.class, ToStringSerializer.instance);

        this.registerModule(simpleModule);
        this.setSerializationInclusion(JsonInclude.Include.NON_EMPTY);
    }
}

```

- springMvc配置，使用CustomObjectMapper

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/mvc
	http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

    <context:component-scan base-package="com.wnx.springmvc" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>


    <bean id="httpMessageConverter" class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
        <!--
            1.使用自定义的ObjectMapper
        -->
        <property name="objectMapper" ref="customObjectMapper"/>
    </bean>
    <!--
        2.注入到消息转换器集合中
    -->
    <mvc:annotation-driven>
        <mvc:message-converters>
            <ref bean="httpMessageConverter"/>
        </mvc:message-converters>
    </mvc:annotation-driven>

    <bean class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <ref bean="dateConverter"/>
            </set>
        </property>
    </bean>
    
    <bean id="templateResolver" class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
        <property name="prefix" value="/WEB-INF/"/>
        <property name="suffix" value=".html"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="order" value="1"/>
        <property name="templateMode" value="HTML"/>
        <property name="cacheable" value="false"/>
    </bean>
    <bean id="templateEngine" class="org.thymeleaf.spring5.SpringTemplateEngine">
        <property name="templateResolver" ref="templateResolver"/>
    </bean>
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="templateEngine" ref="templateEngine"/>
        <property name="characterEncoding" value="UTF-8"/>
    </bean>
    <mvc:resources mapping="/**" location="/"/>

    <bean id="validatorFactoryBean" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
        <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
    </bean>
</beans>
```