# Vue-watch-Check-Username-IS-Available

> 可以利用监听器，来在用户注册的时候，校验用户输入用户名是否可用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js"></script>
</head>
<body>
<div id="app">
   学生姓名： <input v-model="userName"/>
</div>
<script>
    const app = new Vue({
        el:"#app",

        create:{

        },
        data:{
          userName:''

        },
        watch:{
            userName:{
                handler(newValue){
                    if (Object.is(newValue,'')) return
                  $.get('http://localhost:8080/findUser/'+newValue,(result)=>{
                      console.log(result)
                  })
                }
            },

        }
    })
</script>
</body>
</html>
```

```java
package com.wnx.controller;

import com.wnx.api.CommonResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.Objects;

@RestController
public class BackController {

    //1.模拟数据库中有三个用户
    private static final List<String> userList;

    static {
        userList = new ArrayList<>();
        userList.add("a");
        userList.add("ad");
        userList.add("admin");
    }



    @GetMapping("/findUser/{username}")
    public CommonResult<String> findUser(@PathVariable String username){
        CommonResult<String> result = CommonResult.failed("用户名不可用");

        for (String item : userList) {
            if (Objects.equals(username,item)){
                result = CommonResult.success("用户可用");
            }
        }

        return result;
    }

}

```

![image-20220707115134078](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220707115134078.png)