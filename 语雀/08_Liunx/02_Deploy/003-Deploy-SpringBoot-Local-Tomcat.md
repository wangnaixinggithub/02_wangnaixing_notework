# Deploy-SpringBoot-Local-Tomcat

# 1、Update SpringBoot Config

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.1</version>
        <relativePath/>
    </parent>

    <groupId>com.wnx.back</groupId>
    <artifactId>back</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>back</name>
    <description>back</description>
    
    <!--1、改变打包方式为War包-->
    <packaging>war</packaging>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>

        <!--2、在SpringStarter中移除内嵌Tomcat-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        
        <!--3、添加ServletApi-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <scope>provided</scope>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>


        
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

```

# 2、Create Class SpringBootStartApplication

```java
package com.wnx;

import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

public class SpringBootStartApplication extends SpringBootServletInitializer {
    
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        //sources 里面的参数是启动类的类型
        return builder.sources(BackApplication.class);
    }
}
```

> 和启动类同级

![image-20220708234913968](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708234913968.png)

# 3、Upload 

```shell
#1、确保在linux中已经解压好了Tomcat 并进入到webapps目录下
/usr/tomcat/apache-tomcat-10.0.22/webapps

#2、删除ROOT项目
rm -r -f ROOT

#3、将Maven 打包好的工程重新命名为ROOT.war

#4、使用ftp进入上传工作

#5、切换到/usr/tomcat/apache-tomcat-10.0.22/bin
cd /usr/tomcat/apache-tomcat-10.0.22/bin

#6、执行tomcat启动命令
./startup.sh 

```

![image-20220708235350312](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708235350312.png)