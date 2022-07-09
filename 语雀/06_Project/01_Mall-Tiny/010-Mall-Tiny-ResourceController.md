# Mall-Tiny-ResourceController

## 1、根据ID获取资源详情

```java
 	@ApiOperation("根据ID获取资源详情")
    @GetMapping( "/{id}")
    public CommonResult<UmsResource> getItem(@PathVariable Long id) {
        UmsResource umsResource = resourceService.getById(id);
        return CommonResult.success(umsResource);
    }
```

## 2、查询所有后台资源

```java
    @ApiOperation("查询所有后台资源")
    @GetMapping("/listAll")
    public CommonResult<List<UmsResource>> listAll() {
        List<UmsResource> resourceList = resourceService.list();
        return CommonResult.success(resourceList);
    }
```

## 3、分页模糊查询后台资源

```java
    @ApiOperation("分页模糊查询后台资源")
    @GetMapping( "/list")
    public CommonResult<CommonPage<UmsResource>> list(@RequestParam(required = false) Long categoryId,
                                                      @RequestParam(required = false) String nameKeyword,
                                                      @RequestParam(required = false) String urlKeyword,
                                                      @RequestParam(value = "pageSize", defaultValue = "5") Integer pageSize,
                                                      @RequestParam(value = "pageNum", defaultValue = "1") Integer pageNum) {
        Page<UmsResource> resourceList = resourceService.list(categoryId,nameKeyword, urlKeyword, pageSize, pageNum);
        return CommonResult.success(CommonPage.restPage(resourceList));
    }
```

```java
	@Override
    public Page<UmsResource> list(Long categoryId, String nameKeyword, String urlKeyword, Integer pageSize, Integer pageNum) {
        //1.条件查询
        LambdaQueryWrapper<UmsResource> wrapper = new LambdaQueryWrapper<>();

        if(categoryId!=null){
            wrapper.eq(UmsResource::getCategoryId,categoryId);
        }

        if(StrUtil.isNotEmpty(nameKeyword)){
            wrapper.like(UmsResource::getName,nameKeyword);
        }

        if(StrUtil.isNotEmpty(urlKeyword)){
            wrapper.like(UmsResource::getUrl,urlKeyword);
        }

        //2.结果分页
        return page(new Page<>(pageNum,pageSize),wrapper);

    }

```

## 4、添加后台资源

```java
    @ApiOperation("添加后台资源")
    @PostMapping("/create")
    public CommonResult<String> create(@RequestBody UmsResource umsResource) {
        boolean success = resourceService.create(umsResource);

        if (success) {
            return CommonResult.success("添加成功");
        } else {
            return CommonResult.failed();
        }
    }
```

```java
    @Override
    public boolean create(UmsResource umsResource) {
        umsResource.setCreateTime(new Date());
        return save(umsResource);
    }
```

## 5、修改后台资源

```java
    @ApiOperation("修改后台资源")
    @PostMapping("/update/{id}")
    public CommonResult<String> update(@PathVariable Long id,
                               @RequestBody UmsResource umsResource) {
        boolean success = resourceService.update(id, umsResource);

        if (success) {
            return CommonResult.success("修改成功");
        } else {
            return CommonResult.failed();
        }
    }
```

```java
    @Override
    public boolean update(Long id, UmsResource umsResource) {
        umsResource.setId(id);
        boolean success = updateById(umsResource);
        return success;
    }
```

## 6、根据ID删除后台资源

```java
  @ApiOperation("根据ID删除后台资源")
    @PostMapping("/delete/{id}")
    public CommonResult<String> delete(@PathVariable Long id) {
        boolean success = resourceService.delete(id);

        if (success) {
            return CommonResult.success("删除资源成功");
        } else {
            return CommonResult.failed();
        }
    }
```

```java
   @Override
    public boolean delete(Long id) {
        boolean success = removeById(id);
        return  success;
    }
```

