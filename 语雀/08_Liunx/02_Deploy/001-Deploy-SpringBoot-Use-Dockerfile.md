# Deploy-SpringBoot-Use-Dockerfile

> 部署一个SpringBoot项目，无数据库的单体。模型+控制器+视图+静态资源不拦截处理

# 1、Create A SpringBoot Project

```java
package com.wnx.model;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class User implements Serializable {
  private Long id;
  private String name;
  private String phone;
}
```

```java
package com.wnx.controller;

import com.wnx.model.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.servlet.ModelAndView;

import java.util.ArrayList;
import java.util.List;



@Controller
public class BackController {
    private static final List<User> userList;

    static {
        userList = new ArrayList<>();
        userList.add(User.builder().id(1L).name("王乃醒").phone("18154622909").build());
        userList.add(User.builder().id(2L).name("黄洁莹").phone("18154622910").build());
        userList.add(User.builder().id(3L).name("张三").phone("18154622911").build());
        userList.add(User.builder().id(4L).name("李四").phone("18154622912").build());
        userList.add(User.builder().id(5L).name("王五").phone("18154622913").build());
    }

    @GetMapping(value = {"/index","/"})
    public ModelAndView toIndexPage(){
        ModelAndView modelAndView = new ModelAndView("index");
        modelAndView.addObject("userList",userList);
        return modelAndView;

    }


}

```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
    <link href="/static/plugin/bootstrap/css/bootstrap.min.css" rel="stylesheet">
    <script href="/static/plugin/bootstrap/js/bootstrap.bundle.js"></script>
</head>
<body>
  <div class="container">
      <table class="table table-hover table-bordered">
          <thead>
          <tr>
              <th scope="col">ID</th>
              <th scope="col">姓名</th>
              <th scope="col">手机</th>
          </tr>
          </thead>
          <tbody></tbody>
      </table>
  </div>
<script th:inline="javascript" type="module">
    import user from "/static/js/user.js";

    //1、获取数据
    let userList = [[${userList}]]

    //2、渲染数据到HTML视图
    user.renderUserListToHtmlView(userList)
    
</script>
</body>
</html>
```

```js
/** /static/js/user.js
 * 渲染出用户列表到HTML页面
 * @param userList 用户列表
 * @constructor
 */
const renderUserListToHtmlView = userList =>{
    let tbodyElement = document.querySelector('tbody')
    for (let i = 0; i < userList.length; i++) {

        //1、Tr
        let trElement = document.createElement('tr')

        //2、Tr 中 全部 Td
        for (let key in userList[i]){
            let tdElement = document.createElement('td')
            tdElement.innerText = userList[i][key]
            trElement.append(tdElement)
        }

        tbodyElement.append(trElement)


    }
}
export default {
    renderUserListToHtmlView
}
```

```java
package com.wnx.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig  implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**").addResourceLocations("classpath:/static/");
    }
}

```

# 2、Write DockerFile

```dockerfile
# 1、Docker 构建依赖于JDK:8 镜像
FROM java:8

# 2、复制本地所有.jar的文件 到Liunx 根目录/ 下 并把jar文件重命名为app.jar
COPY *.jar /app.jar

# 3、通过CMD 可添加Docker 运行时参数。比如可修改此容器运行时服务端口，默认为8080
CMD ["--server.port=8080"]

# 4、容器对外暴露的端口号为8080
EXPOSE 8080

#5、让容器对app.jar 执行Java -jar 的命令
ENTRYPOINT ["java","-jar","/app.jar"]
```

# 3、Clean And Package Project

执行完成之后，在你的项目下`target` 文件中会得到一个`.jar`文件，这就是打包好的工程应用了。

![image-20220708160122635](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708160122635.png)

# 4、Windows Upload .jar And Dockerfie

```shell
#1、以root账户的身份,远程连接你的服务器

#2、在根目录/下,创建back目录，用于接收 .jar 文件 以及 Dockerfile 文件
cd /
mkdir back
cd  /back

#3、使用Xftp 从Window系统中上传 .jar 以及Dockerfile文件 到 /back 目录下
```

![image-20220708160948631](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708160948631.png)

# 5、Build Customer Docker Images Base On Dockerfile 

```shell
#4、使用 docker build 基于Dockerfile 构建自定义镜像，该镜像名称为 wangNaiXingSpringBootProject001
docker build -t backproject001 .

#5、查看本地镜像仓库，可知已经生成了backproject001 的镜像
docker images

#6、根据此镜像在服务器中运行容器，并且设定为后台启动 把容器内8080端口映射给宿主机（服务器）的8080端口 
docker run -d -p 81:8080 --name wnx-backproject-001 backproject001


#7、获取此容器的本地端口 并使用crul 在此服务器上访问此springboot工程
docker ps 
curl localhost:49153/

#8、防火墙放行81端口，并且服务器安全组也放行81端口
firewall-cmd --zone=public --add-port=81/tcp --permanent

#9、重启防火墙
firewall-cmd --reload
```

![image-20220708163244266](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708163244266.png)

可以看到成功响应了我们HTML页面了！

![image-20220708163323829](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708163323829.png)

可以用浏览器访问到了

```properties
http://47.106.187.56:81
```

![image-20220708193314017](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708193314017.png)