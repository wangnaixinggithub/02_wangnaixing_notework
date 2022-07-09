# Mall-Tiny-UmsMenuController

## 1、添加后台菜单

```java
	@ApiOperation("添加后台菜单")
    @PostMapping("/create")
    public CommonResult<String> create(@RequestBody UmsMenu umsMenu) {
        boolean success = menuService.create(umsMenu);

        if (success) {
            return CommonResult.success("添加成功！");
        } else {
            return CommonResult.failed();
        }
    }
```

```java
    @Override
    public boolean create(UmsMenu umsMenu) {
        umsMenu.setCreateTime(new Date());
        updateLevel(umsMenu);
        return save(umsMenu);
    }
```

```java
	 /**
     * 修改菜单层级
     * @param umsMenu 菜单模型
     */
    private void updateLevel(UmsMenu umsMenu) {
        if (umsMenu.getParentId() == 0) {
            //没有父菜单时为一级菜单
            umsMenu.setLevel(0);
        } else {
            //有父菜单时选择根据父菜单level设置
            UmsMenu parentMenu = getById(umsMenu.getParentId());
            if (parentMenu != null) {
                umsMenu.setLevel(parentMenu.getLevel() + 1);
            } else {
                umsMenu.setLevel(0);
            }
        }
    }
```

## 2、修改后台菜单

```java
    @ApiOperation("修改后台菜单")
    @PostMapping("/update/{id}")
    public CommonResult<String> update(@PathVariable Long id,
                               @RequestBody UmsMenu umsMenu) {
        
        boolean success = menuService.update(id, umsMenu);

        if (success) {
            return CommonResult.success("修改成功！");
        } else {
            return CommonResult.failed();
        }
         
    }
```

```java
    @Override
    public boolean update(Long id, UmsMenu umsMenu) {
        umsMenu.setId(id);
        updateLevel(umsMenu);
        return updateById(umsMenu);
    }
```

## 3、根据ID获取菜单详情

```java
    @ApiOperation("根据ID获取菜单详情")
    @GetMapping("/{id}")
    public CommonResult<UmsMenu> getItem(@PathVariable Long id) {
        UmsMenu umsMenu = menuService.getById(id);
        return CommonResult.success(umsMenu);
    }
```

## 4、根据ID删除后台菜单

```java
    @ApiOperation("根据ID删除后台菜单")
    @PostMapping("/delete/{id}")
    public CommonResult<String> delete(@PathVariable Long id) {
        boolean success = menuService.removeById(id);

        if (success) {
            return CommonResult.success("删除成功！");
        } else {
            return CommonResult.failed();
        }
    }
```

## 5、分页查询后台菜单

```java
    @ApiOperation("分页查询后台菜单")
    @GetMapping("/list/{parentId}")
    public CommonResult<CommonPage<UmsMenu>> list(@PathVariable Long parentId,
                                                  @RequestParam(value = "pageSize", defaultValue = "5") Integer pageSize,
                                                  @RequestParam(value = "pageNum", defaultValue = "1") Integer pageNum) {
        Page<UmsMenu> menuList = menuService.list(parentId, pageSize, pageNum);
        return CommonResult.success(CommonPage.restPage(menuList));
    }
```

```java
    @Override
    public Page<UmsMenu> list(Long parentId, Integer pageSize, Integer pageNum) {
        LambdaQueryWrapper<UmsMenu> wrapper = new LambdaQueryWrapper<>();
      
        wrapper.eq(UmsMenu::getParentId,parentId)
                .orderByDesc(UmsMenu::getSort);
        
        return page(new Page<>(pageNum,pageSize),wrapper);
    }
```

## 6、树形结构返回所有菜单列表

```java
    @ApiOperation("树形结构返回所有菜单列表")
    @GetMapping("/treeList")
    public CommonResult<List<UmsMenuNode>> treeList() {
        List<UmsMenuNode> list = menuService.treeList();
        return CommonResult.success(list);
    }
```

```java
    @Override
    public List<UmsMenuNode> treeList() {
        List<UmsMenu> menuList = list();
        List<UmsMenuNode> result = menuList.stream()
                .filter(menu -> menu.getParentId().equals(0L))
                .map(menu -> covertMenuNode(menu, menuList))
            .collect(Collectors.toList());
        return result;
    }
```

```java
	 /**
     * 将UmsMenu转化为UmsMenuNode并设置children属性
     */
    private UmsMenuNode covertMenuNode(UmsMenu menu, List<UmsMenu> menuList) {
        UmsMenuNode node = new UmsMenuNode();
        BeanUtils.copyProperties(menu, node);

        List<UmsMenuNode> children = menuList.stream()
                .filter(subMenu -> subMenu.getParentId().equals(menu.getId()))
                .map(subMenu -> covertMenuNode(subMenu, menuList))
                .collect(Collectors.toList());

        node.setChildren(children);

        return node;
    }
```

## 7、修改菜单显示状态

```java
    @ApiOperation("修改菜单显示状态")
    @PostMapping("/updateHidden/{id}")
    public CommonResult<String> updateHidden(@PathVariable Long id, @RequestParam("hidden") Integer hidden) {
        boolean success = menuService.updateHidden(id, hidden);

        if (success) {
            return CommonResult.success("修改成功！");
        } else {
            return CommonResult.failed();
        }
    }
```

```java
    @Override
    public boolean updateHidden(Long id, Integer hidden) {
        UmsMenu umsMenu = new UmsMenu();
        umsMenu.setId(id);
        umsMenu.setHidden(hidden);
        return updateById(umsMenu);
    }
```

