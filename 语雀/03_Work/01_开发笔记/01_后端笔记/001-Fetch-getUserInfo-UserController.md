# 在开发中如何获取用户的相关信息？

# 1、UserService

```java
public interface UserService {
	//1.获取当前线程绑定的用户Id
    String getUserId();
 	
    //2.根据用户ID获取用户信息
    UserInfoVO getUserInfoById(String userId);
	
	
    //获取用户的角色信息
    List<AppRole> getUserAppRoleById(String userId);
    
	//批量新增用户
    boolean[] addUsers(List<User> users);
    
	//修改用户信息
    boolean updateUser(User user);
    
    //批量删除用户信息
    boolean[] deleteUsers(String[] var1);
    

	//根据登陆名获取用户信息
    User queryUserByFullName(String loginName);
    
	//根据id查询用户信息
    User queryUser(String id);
    
	// 检查用户密码
    boolean checkUserPw(String userId, String pwd);
    
    // 设置用户密码
    boolean setUserPw(String userId, String pwd);
    
    // 修改用户密码
    boolean changeUserPwd(String userId, String pwd);
    

}
```

# 2、POJO-User

- 针对业务方法，我们看看传输数据的POJO类，里面记录了User类有的字段信息,这些信息和数据库字段一一对应。

```java
@Getter
@Setter
@ToString
public class User extends DynamicBean implements IdentityModel {    
	
    //用户唯一标识
    private String id;
    
	//标准组织单元唯一标识
    private String baseOrgUnitId;
	
    //用户密码
    private String pwd;
 	
    //用户名
    private String name;
	
    //用户姓名
    private String fullName;
  	
    //用户状态
     private int status;
    
	//用户编号
    private String number;
	
    //用户类型
    private int type;
	
    //允许访问网络类型
    private int netType;
	
    //用户生日
    private Date birth;
	
    //用户性别
    private int sex;
	
    //用户身份证号
    private String card;
	
    //用户手机号
    private String phone;
	
    //用户邮箱
    private String email;
	
    //用户解锁时间
    private Timestamp unlockTime;
	
    //允许登录的IP段
    private String ip;
	
    //允许登录的起始时间
    private Timestamp stime;
 	
    //允许登录的终止时间
    private Timestamp etime;
	
    //用户创建时间
    private Timestamp ctime;
	
 	//用户上一次修改密码时间
    private Timestamp pmtime;
 	
    //用户上一次登录时间
    private Timestamp ltime;
	
    //用户扩展字段 JSON的Map
    private String ext;
 	
    //用户图片
    private String userImage;
  	
    //用户前三次修改的密码
    private String oldThreePassword;
    
    //登录Token的处理，和用户权限有关系，并不存到数据库。还提供了登录Token的处理。
    private String token;
    private String refreshToken;
    
   }
```

- 解析用户扩展字段。

```java
public class User extends DynamicBean implements IdentityModel {  
    		
		public Map<String, Object> getExtMap() {
        		
         Map<String, Object> extMap = new HashMap();  
        if (StringUtil.isNullOrEmpty(this.getExt())) {
            return extMap;
        } else {
            try {
                //将JsonStr => Map 并返回。
                return (Map)JsonUtil.json2Obj(this.getExt(), Map.class);
            
                } catch (Exception var3) {
               
                log.error("解析用户的扩展字段extMap出错，用户id:{} 用户名:{}", this.getId(), this.getName());
                return extMap;
            }
        }
    }
}
```

- 重写了toString()方法，输出只是输出部分信息.

```java
   public class User extends DynamicBean implements IdentityModel {   
   public String toString() {
        StringBuilder sb = new StringBuilder("User{");
        sb.append("id='").append(this.id).append('\'');
        sb.append(", baseOrgUnitId='").append(this.baseOrgUnitId).append('\'');
        sb.append(", name='").append(this.name).append('\'');
        sb.append(", fullName='").append(this.fullName).append('\'');
        sb.append(", pwd='").append(this.pwd).append('\'');
        sb.append('}');
        return sb.toString();
    }
}
```

# 3、VO-UserInfoVO

- 对外暴露Vo,只提供很少的信息。

```java
@Data
@NoArgsConstructor
public class UserInfoVO {
    //用户唯一标识
    private String userId;
    
    //用户账号
    private String userName;
    
    // 用户所属单位
    private String orgName;
    
    // 用户所属单位编码
    private String orgCode;
    
    // 用户所属部门（有可能是上级部门）
    private String dept;
    
    // 用户直属部门
    private String userDept;
    
    // 用户直属部门ID
    private String userDeptId;
    
    // 用户角色ID
    private List<String> userRoleIds;
    
    @JsonIgnore
    private String ipAddress;
    
    private Map<String, Object> userExt;
    
    public UserInfoVO(User user) {
        this.userId = user.getId();
        this.userName = user.getName();
    }
}

```

# 4、Business

```java
        String userId = userService.getUserId();
        UserInfoVO userInfo = userService.getUserInfoById(userId);
```



