# Mall-Tiny-UmsRoleController

## 1、添加角色

```java
    @ApiOperation("添加角色")
    @PostMapping("/create")
    public CommonResult<String> create(@RequestBody UmsRole role) {
        boolean success = roleService.create(role);
        if (success) {
            return CommonResult.success("添加成功！");
        }
        return CommonResult.failed();
    }
```

```java
    @Override
    public boolean create(UmsRole role) {
        role.setCreateTime(new Date());
        role.setAdminCount(0);
        role.setSort(0);
        return save(role);
    }
```

## 2、修改角色

```java
 	@ApiOperation("修改角色")
    @PostMapping("/update/{id}")
    public CommonResult<String> update(@PathVariable Long id, @RequestBody UmsRole role) {
        role.setId(id);
        boolean success = roleService.updateById(role);
        if (success) {
            return CommonResult.success("修改成功");
        }
        return CommonResult.failed();
    }
```

## 3、批量删除角色

```java
    @ApiOperation("批量删除角色")
    @PostMapping( "/delete")
    public CommonResult<String> delete(@RequestParam("ids") List<Long> ids) {

        boolean success = roleService.delete(ids);

        if (success) {
            return CommonResult.success("删除成功！");
        }
        return CommonResult.failed();
    }
```

```java
    @Override
    public boolean delete(List<Long> ids) {
        boolean success = removeByIds(ids);
        return success;
    }
```

## 4、获取所有角色

```java
    @ApiOperation("获取所有角色")
    @GetMapping("/listAll")
    public CommonResult<List<UmsRole>> listAll() {
        List<UmsRole> roleList = roleService.list();
        return CommonResult.success(roleList);
    }
```

## 5、根据角色名称分页获取角色列表

```java
    @ApiOperation("根据角色名称分页获取角色列表")
    @GetMapping("/list")
    public CommonResult<CommonPage<UmsRole>> list(@RequestParam(value = "keyword", required = false) String keyword,
                                                  @RequestParam(value = "pageSize", defaultValue = "5") Integer pageSize,
                                                  @RequestParam(value = "pageNum", defaultValue = "1") Integer pageNum) {
        Page<UmsRole> roleList = roleService.list(keyword, pageSize, pageNum);
        return CommonResult.success(CommonPage.restPage(roleList));
    }
```

```java
    @Override
    public Page<UmsRole> list(String keyword, Integer pageSize, Integer pageNum) {
        LambdaQueryWrapper<UmsRole> wrapper = new LambdaQueryWrapper<>();

        if(StrUtil.isNotEmpty(keyword)){
            wrapper.like(UmsRole::getName,keyword);
        }

        return page(new Page<>(pageNum,pageSize),wrapper);
    }
```

## 6、修改角色状态

```java
 	@ApiOperation("修改角色状态")
    @PostMapping( "/updateStatus/{id}")
    public CommonResult<String> updateStatus(@PathVariable Long id, @RequestParam(value = "status") Integer status) {
        UmsRole umsRole = new UmsRole();
        umsRole.setId(id);
        umsRole.setStatus(status);
        boolean success = roleService.updateById(umsRole);

        if (success) {
            return CommonResult.success("修改成功！");
        }

        return CommonResult.failed();
    }
```

## 7、获取角色相关菜单

```java
    @ApiOperation("获取角色相关菜单")
    @GetMapping("/listMenu/{roleId}")
    public CommonResult<List<UmsMenu>> listMenu(@PathVariable Long roleId) {
        List<UmsMenu> roleList = roleService.listMenu(roleId);
        return CommonResult.success(roleList);
    }
```

```java
   @Override
    public List<UmsMenu> listMenu(Long roleId) {
        return menuMapper.getMenuListByRoleId(roleId);
    }
```

```sql
    <select id="getMenuListByRoleId" resultType="com.wnx.mall.tiny.modules.ums.model.UmsMenu">
        SELECT
            m.id id,
            m.parent_id parentId,
            m.create_time createTime,
            m.title title,
            m.level level,
            m.sort sort,
            m.name name,
            m.icon icon,
            m.hidden hidden
        FROM
            ums_role_menu_relation rmr
                LEFT JOIN ums_menu m ON rmr.menu_id = m.id
        WHERE
            rmr.role_id = #{roleId}
          AND m.id IS NOT NULL
        GROUP BY
            m.id
    </select>
```

## 8、获取角色相关资源

```java
    @ApiOperation("获取角色相关资源")
    @GetMapping("/listResource/{roleId}")
    public CommonResult<List<UmsResource>> listResource(@PathVariable Long roleId) {
        List<UmsResource> roleList = roleService.listResource(roleId);
        return CommonResult.success(roleList);
    }
```

```java
    @Override
    public List<UmsResource> listResource(Long roleId) {
        return resourceMapper.getResourceListByRoleId(roleId);
    }
```

```sql
    <select id="getResourceListByRoleId" resultType="com.wnx.mall.tiny.modules.ums.model.UmsResource">
        SELECT
            r.id id,
            r.create_time createTime,
            r.`name` `name`,
            r.url url,
            r.description description,
            r.category_id categoryId
        FROM
            ums_role_resource_relation rrr
                LEFT JOIN ums_resource r ON rrr.resource_id = r.id
        WHERE
            rrr.role_id = #{roleId}
          AND r.id IS NOT NULL
        GROUP BY
            r.id
    </select>
```

## 9、给角色分配菜单

```java
    @ApiOperation("给角色分配菜单")
    @PostMapping("/allocMenu")
    public CommonResult<Integer> allocMenu(@RequestParam Long roleId, @RequestParam List<Long> menuIds) {
        int count = roleService.allocMenu(roleId, menuIds);
        return CommonResult.success(count);
    }
```

```java
  @Override
    public int allocMenu(Long roleId, List<Long> menuIds) {
        //先删除原有关系
        QueryWrapper<UmsRoleMenuRelation> wrapper = new QueryWrapper<>();
        wrapper.lambda().eq(UmsRoleMenuRelation::getRoleId,roleId);
        roleMenuRelationService.remove(wrapper);

        //批量插入新关系
        List<UmsRoleMenuRelation> relationList = new ArrayList<>();
        for (Long menuId : menuIds) {
            UmsRoleMenuRelation relation = new UmsRoleMenuRelation();
            relation.setRoleId(roleId);
            relation.setMenuId(menuId);
            relationList.add(relation);
        }
        roleMenuRelationService.saveBatch(relationList);

        return menuIds.size();
    }

```

## 10、给角色分配资源

```java
    @ApiOperation("给角色分配资源")
    @PostMapping("/allocResource")
    public CommonResult<Integer> allocResource(@RequestParam Long roleId, @RequestParam List<Long> resourceIds) {
        int count = roleService.allocResource(roleId, resourceIds);
        return CommonResult.success(count);
    }
```

```java
 @Override
    public int allocResource(Long roleId, List<Long> resourceIds) {
        //先删除原有关系
        QueryWrapper<UmsRoleResourceRelation> wrapper = new QueryWrapper<>();
        wrapper.lambda().eq(UmsRoleResourceRelation::getRoleId,roleId);
        roleResourceRelationService.remove(wrapper);
        //批量插入新关系
        List<UmsRoleResourceRelation> relationList = new ArrayList<>();
        for (Long resourceId : resourceIds) {
            UmsRoleResourceRelation relation = new UmsRoleResourceRelation();
            relation.setRoleId(roleId);
            relation.setResourceId(resourceId);
            relationList.add(relation);
        }
        roleResourceRelationService.saveBatch(relationList);
        return resourceIds.size();
    }
```

