# 001-Fetch-getUserInfo-UserController-02

# 1、UserController#fetchUserInfo => user/info

```java
@Slf4j
@Api(tags = "用户相关API")
@RestController
@RequestMapping(path = "/api/v1/")
public class UserController {

    @Autowired
    private UserService userService;

    @RateLimit
    @ApiOperation(value = "获取用户信息")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "fetchType", value = "查询类型，basic:基础查询", paramType = "query", required = false)
    })
    @GetMapping("user/info")
    public UserInfoVO fetchUserInfo(HttpServletRequest request,
                                    @RequestParam(value = "fetchType", defaultValue = "basic") String fetchType) {
        //1.从UserService中获取当前线程绑定的用户ID
        String userId = userService.getUserId();
        
		//2.调用UserService业务层，根据用户ID获取用户信息，得到用户信息Vo
        UserInfoVO infoVo = userService.getUserInfoById(userId);
        
        //3.使用IP地址工具类，设置进去IP地址
        infoVo.setIpAddress(IPAddressUtil.getIPAddress(request));
        
        //4.并直接返回UserInfo的JSON字符串。
        return infoVo;
    }
}
```

# 2、UserServiceImpl

```java
@Slf4j
@Service
public class UserServiceImpl implements UserService {
    
	@HessianClient(ServiceNames.UA_WEB)
    private IUserBizc userBizc;
    
    @Cacheable(value = Constants.USER_BASIC_INFO_CACHE_NAME)  //userBasicInfoCacheManager
   
    @Override
    public UserInfoVO getUserInfoById(String userId) {
        //注入了userBizc，根据用户ID得到用户信息
        User user = userBizc.getUser(userId);
        
        if (Objects.isNull(user)) {
            ErrorInfo info = new ErrorInfo(ErrorCode.not_found.name(), ErrorDescription.RECORD_NOT_EXIST);
            throw new WebServerException(HttpStatus.NOT_FOUND, info, "数据库中不存在UserId为" + userId + "的用户信息");
        }
        
        //包装User信息Vo
        UserInfoVO userInfoVo = new UserInfoVO(user);
        
        //很多逻辑。。。。
    }  
}
```

# 3、@HessianClient

```java
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Autowired
public @interface HessianClient {
    String value();

    String scheme() default "http";

    String contextPath() default "";

    long timeout() default 0L;
}
```

# 4、How Use By Back ?

```java
 String userId = userService.getUserId();
 UserInfoVO infoVo = userService.getUserInfoById(userId);
```

# 5、VUE Use ?

## 1、dataApi.js

```java
import $http from "@/common/utils/http";

const getUserInfo = ()=> $http.get('user/info');

const dataApi = {
	getUserInfo
}
export default dataApi;
```

## 2、business

```vue
<script>
methods:{
	  async getUser(){
          		//1.from cache
                let user = this.$cache.userInfo;
          
          		//2.from sessionStorage
                let session = window.sessionStorage;
          
          	
                if(!user || !user.userId || !user.userName){
                    user = (session.getItem("user") && session.getItem("user").includes("userId")) ?
                        JSON.parse(session.getItem("user")) : undefined;
                }
       
          
                if(this.user && this.user.userId && this.user.userName) {
                    //set cache
                    this.$cache.userInfo = user;
                }else{ 
                    //3.from dataApi
                    user = await dataApi.getUserInfo();
                    
                     if(user){
                      	
                         // set cache And Session
                        this.$cache.userInfo = user;
                   
                        session.setItem("user", JSON.stringify(user));
                    }else{
                        
                        console.error("获取用户信息失败！");
                    }
                } 
            }
}
</script>
```

## 3、cache.js/cache.userInfo

```java
const cache = {
    
    // cache Object.
    userInfo:{
        userId: undefined,
        userName: undefined,
        orgCode: undefined,
        orgName: undefined,
        dept: undefined
    },
    
    
     // provider getUserInfo Method!
     getUserInfo(){
         //1.from cache
        if(this.userInfo && this.userInfo.userId) {
            return this.userInfo;
        }else{
            //2.from session
            let session = window.sessionStorage;
            let user = (session.getItem("user") && session.getItem("user").includes("userId")) ?
                JSON.parse(session.getItem("user")) : undefined;
            
            if(user && user.userId){
                this.userInfoPending = false;
                this.userInfo = user;
            }

      
            if(this.userInfoPending == false){
                this.userInfoPending = true;
                
                http.get("user/info", null).then(res=>{
                        this.userInfoPending = false;
                        this.userInfo = res;
                        let session = window.sessionStorage;
                        session.setItem("user", JSON.stringify(res));
                        return this.userInfo;
                    }).catch(res=>{
                        this.userInfoPending = false;
                });
                
            }else{
                setTimeout(this.getUserInfo, 200);
            }

        }
    },


};
export default cache;
```

## 4、main.js

```javascript
import cache from "./utils/cache.js";
Vue.prototype.$cache = cache;
```

- 不理解的代码

```javascript
async getUser(){
	await dataApi.getUserInfo();
}
```

> 可以在控制台应用中的会话存储找到item=User，value=用户相关信息的JSON

![image-20220420151316137](001-Fetch-getUserInfo-UserController-02.assets/image-20220420151316137.png)
