# Use-Stream-get-PermissionTree

我们经常会有返回树形结构数据的需求。比如这里的权限，第一层是目录权限，目录权限之下有菜单权限，菜单权限之下有按钮权限。如果我们要返回一个集合，包含目录权限，目录权限下面嵌套菜单权限，菜单权限下嵌套按钮权限。使用Stream API可以很方便的解决这个问题。

```java

public class UmsPermission implements Serializable {
    @ApiModelProperty(value = "自增主键")
    private Long id;

    @ApiModelProperty(value = "父级权限ID")
    private Long pid;

    @ApiModelProperty(value = "名称")
    private String name;

    @ApiModelProperty(value = "权限标识符")
    private String value;

    @ApiModelProperty(value = "图标")
    private String icon;

    @ApiModelProperty(value = "权限类型：0->目录 1->菜单 2->按钮（接口绑定权限）")
    private Integer type;

    @ApiModelProperty(value = "前端资源路径")
    private String uri;

    @ApiModelProperty(value = "启动状态：0->禁用 1->启用")
    private Integer status;

    @ApiModelProperty(value = "创建时间")
    private Date createTime;

    @ApiModelProperty(value = "排序号")
    private Integer sort;

  //省略所有getter及setter方法
    
}
```

```java

publicclass UmsPermissionNode extends UmsPermission {
    private List<UmsPermissionNode> children;

    public List<UmsPermissionNode> getChildren() {
        return children;
    }

    public void setChildren(List<UmsPermissionNode> children) {
        this.children = children;
    }
}

```

我们先过滤出pid为0的顶级权限，然后给每个顶级权限设置其子级权限，`covert方法`的主要用途就是从所有权限中找出相应权限的子级权限。

```java
@Override
public List<UmsPermissionNode> treeList() {
    //查询全部权限
    List<UmsPermission> permissionList = permissionMapper.selectByExample(new UmsPermissionExample());
 
    List<UmsPermissionNode> result = permissionList.stream()
            .filter(permission -> permission.getPid().equals(0L))   //过滤出pid=0的权限
            .map(permission -> covert(permission, permissionList))  //每一个permission 继续调用convert()方法进行转换。
            .collect(Collectors.toList()); //收集流成集合
    
    return result;
}
```

- 这里把递归调用方法放到了map操作中去，当没有子级权限时filter下的map操作便不会再执行，从而停止递归

```java
/**
* 将权限转换为带有子级的权限对象
* 当找不到子级权限的时候map操作不会再递归调用covert
*/
private UmsPermissionNode covert(UmsPermission permission, List<UmsPermission> permissionList) {
   
    UmsPermissionNode node = new UmsPermissionNode();
   
    BeanUtils.copyProperties(permission, node);
   
    List<UmsPermissionNode> children = permissionList.stream()
           .filter(subPermission -> subPermission.getPid().equals(permission.getId()))
           .map(subPermission -> covert(subPermission, permissionList)).collect(Collectors.toList());
   
    node.setChildren(children);
    
    return node;
    
}
```

