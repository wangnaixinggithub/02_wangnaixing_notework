# Mall-Tiny-UmsResourceCategoryController

## 1、查询所有后台资源分类

```java
    @ApiOperation("查询所有后台资源分类")
    @GetMapping("/listAll")
    public CommonResult<List<UmsResourceCategory>> listAll() {
        List<UmsResourceCategory> resourceList = resourceCategoryService.listAll();
        return CommonResult.success(resourceList);
    }
```

```java
    @Override
    public List<UmsResourceCategory> listAll() {
        LambdaQueryWrapper<UmsResourceCategory> wrapper = new LambdaQueryWrapper<>();
        wrapper.orderByDesc(UmsResourceCategory::getSort);
        return list(wrapper);
    }
```

## 2、添加后台资源分类

```java
    @ApiOperation("添加后台资源分类")
    @PostMapping("/create")
    public CommonResult<String> create(@RequestBody UmsResourceCategory umsResourceCategory) {
        
        boolean success = resourceCategoryService.create(umsResourceCategory);

        if (success) {
            return CommonResult.success("添加成功");
        } else {
            return CommonResult.failed();
        }
    }
```

```java
   @Override
    public boolean create(UmsResourceCategory umsResourceCategory) {
        umsResourceCategory.setCreateTime(new Date());
        return save(umsResourceCategory);
    }
```

## 3、修改后台资源分类

```java
    @ApiOperation("修改后台资源分类")
    @PostMapping("/update/{id}")
    @ResponseBody
    public CommonResult<String> update(@PathVariable Long id,
                               @RequestBody UmsResourceCategory umsResourceCategory) {
        umsResourceCategory.setId(id);
        boolean success = resourceCategoryService.updateById(umsResourceCategory);

        if (success) {
            return CommonResult.success("添加成功");
        } else {
            return CommonResult.failed();
        }
    }
```

## 4、根据ID删除后台资源分类

```java
    @ApiOperation("根据ID删除后台资源分类")
    @GetMapping("/delete/{id}")
    public CommonResult<String> delete(@PathVariable Long id) {
        
        boolean success = resourceCategoryService.removeById(id);

        if (success) {
            return CommonResult.success("删除成功！");
        } else {
            return CommonResult.failed();
        }
    }
```

