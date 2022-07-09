# Mall-Tiny-UmsAdminController

## 1、用户注册

```java
    @ApiOperation(value = "用户注册")
    @PostMapping("/register")
    public CommonResult<UmsAdmin> register(@Validated @RequestBody UmsAdminParam umsAdminParam) {
        
        UmsAdmin umsAdmin = adminService.register(umsAdminParam);

        if (umsAdmin == null) {
            return CommonResult.failed();
        }
        
        return CommonResult.success(umsAdmin);
    }
```

```java
 	@Override
    public UmsAdmin register(UmsAdminParam umsAdminParam) {
    
    }
```

## 2、登录以后返回token

```yaml
jwt:
  tokenHead: 'Bearer ' 
```

```java

    @Value("${jwt.tokenHead}")
    private String tokenHead;	

    @ApiOperation(value = "登录以后返回token")
    @PostMapping("/login")
    public CommonResult<Map<String,String>> login(@Validated @RequestBody UmsAdminLoginParam umsAdminLoginParam) {
      
        String token = adminService.login(umsAdminLoginParam.getUsername(), umsAdminLoginParam.getPassword());

        if (token == null) {
            return CommonResult.validateFailed("用户名或密码错误");
        }

        Map<String, String> tokenMap = new HashMap<>();
        
        tokenMap.put("token", token);
        tokenMap.put("tokenHead", tokenHead);
       
        return CommonResult.success(tokenMap);
    }
```

## 3、刷新token

```yaml
jwt:
  tokenHeader: Authorization
```

```java
   	@Value("${jwt.tokenHeader}")
    private String tokenHeader;

	@ApiOperation(value = "刷新token")
    @GetMapping( "/refreshToken")
    public CommonResult<Map<String,String>> refreshToken(HttpServletRequest request) {
        String token = request.getHeader(tokenHeader);
        String refreshToken = adminService.refreshToken(token);

        if (refreshToken == null) {
            return CommonResult.failed("token已经过期！");
        }

        Map<String, String> tokenMap = new HashMap<>();
        tokenMap.put("token", refreshToken);
        tokenMap.put("tokenHead", tokenHead);
       
        return CommonResult.success(tokenMap);
    }
```

```java
    @Override
    public String refreshToken(String oldToken) {
        return "1234567890-";
    }
```

## 4、获取当前登录用户信息

```java
    @ApiOperation(value = "获取当前登录用户信息")
    @GetMapping(value = "/info")
    public CommonResult<Map<String, Object>> getAdminInfo(Principal principal) {
        Map<String, Object> data = new HashMap<>();
        
        if(principal==null){
            return CommonResult.unauthorized(null);
        }
        
        String username = principal.getName();
        UmsAdmin umsAdmin = adminService.getAdminByUsername(username);
        
     
        
        data.put("username", umsAdmin.getUsername());
        
        data.put("menus", roleService.getMenuList(umsAdmin.getId()));
        
        data.put("icon", umsAdmin.getIcon());
        
        List<UmsRole> roleList = adminService.getRoleList(umsAdmin.getId());
        
        if(CollUtil.isNotEmpty(roleList)){
            List<String> roles = roleList.stream()
                    .map(UmsRole::getName)
                    .collect(Collectors.toList());
            data.put("roles",roles);
        }

        return CommonResult.success(data);
    }
```

```java
    //根据用户名获取用户信息
	UmsAdmin getAdminByUsername(String username);
	
	//根据用户ID获取角色信息
    List<UmsRole> getRoleList(Long id);
	
	//根据用户ID获取菜单信息
    List<UmsMenu> getMenuList(Long id);
```

## 5、登出功能

```java
    @ApiOperation(value = "登出功能")
    @PostMapping("/logout")
    public CommonResult<String> logout() {
        return CommonResult.success("登出成功！");
    }
```

## 6、根据用户名或姓名分页获取用户列表

```java
 	@ApiOperation("根据用户名或姓名分页获取用户列表")
    @GetMapping("/list")
    public CommonResult<CommonPage<UmsAdmin>> list(@RequestParam(value = "keyword", required = false) String keyword,
                                                   @RequestParam(value = "pageSize", defaultValue = "5") Integer pageSize,
                                                   @RequestParam(value = "pageNum", defaultValue = "1") Integer pageNum) {
        Page<UmsAdmin> adminList = adminService.list(keyword, pageSize, pageNum);
        return CommonResult.success(CommonPage.restPage(adminList));
    }
```

```java
    @Override
    public Page<UmsAdmin> list(String keyword, Integer pageSize, Integer pageNum) {
        LambdaQueryWrapper<UmsAdmin> wrapper = new LambdaQueryWrapper<>();

        if(StrUtil.isNotEmpty(keyword)){
            wrapper.like(UmsAdmin::getUsername,keyword);
            wrapper.or().like(UmsAdmin::getNickName,keyword);
        }

        return page(new Page<>(pageNum,pageSize),wrapper);
    }

```

## 7、获取指定用户信息

```java
    @ApiOperation("获取指定用户信息")
    @GetMapping("/{id}")
    public CommonResult<UmsAdmin> getItem(@PathVariable Long id) {
        UmsAdmin admin = adminService.getById(id);
        return CommonResult.success(admin);
    }
```

## 8、修改指定用户信息

```java
    @ApiOperation("修改指定用户信息")
    @PostMapping("/update/{id}")
    public CommonResult<String> update(@PathVariable Long id, @RequestBody UmsAdmin admin) {
        boolean success = adminService.update(id, admin);

        if (success) {
            return CommonResult.success(null);
        }
        return CommonResult.failed();
    }
```

```java
    @Override
    public boolean update(Long id, UmsAdmin admin) {
        admin.setId(id);
        UmsAdmin rawAdmin = getById(id);
        if(rawAdmin.getPassword().equals(admin.getPassword())){
            //与原加密密码相同的不需要修改
            admin.setPassword(null);
        }else{
            //与原加密密码不同的需要加密修改
            if(StrUtil.isEmpty(admin.getPassword())){
                admin.setPassword(null);
            }else{
               // admin.setPassword(passwordEncoder.encode(admin.getPassword()));
            }
        }
        boolean success = updateById(admin);

        return success;
    }
```

## 9、删除指定用户信息

```java
 @ApiOperation("删除指定用户信息")
    @PostMapping("/delete/{id}")
    public CommonResult delete(@PathVariable Long id) {
        boolean success = adminService.delete(id);
        if (success) {
            return CommonResult.success(null);
        }
        return CommonResult.failed();
    }
```

```java
    @Override
    public boolean delete(Long id) {
        boolean success = removeById(id);
        return success;
    }
```

## 10、修改帐号状态

```java
    @ApiOperation("修改帐号状态")
    @PostMapping("/updateStatus/{id}")
    public CommonResult updateStatus(@PathVariable Long id,@RequestParam(value = "status") Integer status) {
        UmsAdmin umsAdmin = new UmsAdmin();
        umsAdmin.setStatus(status);
        boolean success = adminService.update(id,umsAdmin);
        
        if (success) {
            return CommonResult.success(null);
        }
        return CommonResult.failed();
    }
```

```java
    @Override
    public boolean update(Long id, UmsAdmin admin) {
        admin.setId(id);
        UmsAdmin rawAdmin = getById(id);
        if(rawAdmin.getPassword().equals(admin.getPassword())){
            //与原加密密码相同的不需要修改
            admin.setPassword(null);
        }else{
            //与原加密密码不同的需要加密修改
            if(StrUtil.isEmpty(admin.getPassword())){
                admin.setPassword(null);
            }else{
               // admin.setPassword(passwordEncoder.encode(admin.getPassword()));
            }
        }
        boolean success = updateById(admin);

        return success;
    }
```

## 11、给用户分配角色

```java
    @ApiOperation("给用户分配角色")
    @RequestMapping(value = "/role/update", method = RequestMethod.POST)
    public CommonResult updateRole(@RequestParam("adminId") Long adminId,
                                   @RequestParam("roleIds") List<Long> roleIds) {
        int count = adminService.updateRole(adminId, roleIds);
      
        if (count >= 0) {
            return CommonResult.success(count);
        }
        
        return CommonResult.failed();
    }
```

```java
   @Override
    public int updateRole(Long adminId, List<Long> roleIds) {
        int count = roleIds == null ? 0 : roleIds.size();
        //先删除原来的关系
        QueryWrapper<UmsAdminRoleRelation> wrapper = new QueryWrapper<>();
        wrapper.lambda().eq(UmsAdminRoleRelation::getAdminId,adminId);
        adminRoleRelationService.remove(wrapper);

        //建立新关系
        if (!CollectionUtils.isEmpty(roleIds)) {
            List<UmsAdminRoleRelation> list = new ArrayList<>();
            for (Long roleId : roleIds) {
                UmsAdminRoleRelation roleRelation = new UmsAdminRoleRelation();
                roleRelation.setAdminId(adminId);
                roleRelation.setRoleId(roleId);
                list.add(roleRelation);
            }
            adminRoleRelationService.saveBatch(list);
        }

        return count;
    }
```

## 12、获取指定用户的角色

```java
    @ApiOperation("获取指定用户的角色")
    @GetMapping("/role/{adminId}")
    public CommonResult<List<UmsRole>> getRoleList(@PathVariable Long adminId) {
        List<UmsRole> roleList = adminService.getRoleList(adminId);
        return CommonResult.success(roleList);
    }

```

```java
    @Override
    public List<UmsRole> getRoleList(Long adminId) {
        return roleMapper.getRoleList(adminId);
    }
```

```sql
    <select id="getRoleList" resultType="com.wnx.mall.tiny.modules.ums.model.UmsRole"
            parameterType="java.lang.Long">
        select r.*
        from ums_admin_role_relation ar left join ums_role r on ar.role_id = r.id
        where ar.admin_id = #{adminId}
    </select>
```

