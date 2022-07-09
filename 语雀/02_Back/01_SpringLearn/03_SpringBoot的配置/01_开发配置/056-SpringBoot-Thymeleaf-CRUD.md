# SpringBoot-Thymeleaf-CRUD



```java
package com.example.manager.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig  implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        System.out.println("==========静态资源拦截！============");
        //将所有/static/** 访问都映射到classpath:/static/ 目录下
        registry.addResourceHandler("/static/**").addResourceLocations("classpath:/static/");
    }

}
```

- 页面控制器

```java
@RestController
public class PageController {
    @Resource
    private PmsBrandService pmsBrandService;


    @RequestMapping("/brands")
    public ModelAndView toBrands(){
       ModelAndView view = new ModelAndView("brand/list");
       view.addObject("itemList",pmsBrandService.findAll());
       return view;

    }
}

```

- 业务控制器

```java

@RestController
@RequestMapping(value = "/brand")
public class PmsBrandController {
    @Resource
    private PmsBrandService brandService;

    @PostMapping("/insert")
    public ModelAndView insert(PmsBrand pmsBrand){
        brandService.insert(pmsBrand);
        ModelAndView view = new ModelAndView("brand/list::displayTable");
        view.addObject("itemList",brandService.findAll());
      
        return view;
    }
    @GetMapping("/delete/{id}")
    public ModelAndView delete(@PathVariable(name = "id")Long id){
        brandService.delete(id);
        ModelAndView view = new ModelAndView("brand/list::displayTable");
        view.addObject("itemList",brandService.findAll());
        
        return view;
    }
    @PostMapping("/update")
    public ModelAndView update(PmsBrand pmsBrand){
        brandService.update(pmsBrand);
        ModelAndView view = new ModelAndView("brand/list::displayTable");
        view.addObject("itemList",brandService.findAll());
       
        return view;
    }
    @GetMapping("/select")
    public ModelAndView select(String search){
        ModelAndView view = new ModelAndView("brand/list::displayTable");
        view.addObject("itemList",brandService.selectLike(search));
        
        return view;
    }


}

```

# 1、Front View

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="author" content="Yuan-Programmer" />
    <title>后台管理</title>
    <link href="/static/css/bootstrap.min14ed.css?v=3.3.6" rel="stylesheet">
    <link href="/static/css/font-awesome.min93e3.css?v=4.4.0" rel="stylesheet">
    <link href="/static/css/plugins/iCheck/custom.css" rel="stylesheet">
    <link href="/static/css/animate.min.css" rel="stylesheet">
    <link href="/static/css/style.min862f.css?v=4.1.0" rel="stylesheet">
    <script src="/static/js/jquery-3.4.1/jquery-3.4.1.js"></script>
</head>
<body class="gray-bg">
<div class="wrapper wrapper-content animated fadeInRight">
    <div class="row">
        <div class="col-sm-6">
            <div class="ibox float-e-margins">
                <div class="ibox-title">
                    <h5>品牌管理</h5>
                    <div class="ibox-tools">
                        <button id="addBtn" class="btn btn-xs btn-rounded btn-primary"><i class="fa fa-plus"></i> 添加</button>
                    </div>
                </div>
                <div class="ibox-content">
                    <div class="input-group">
                        <input id="search" type="text" placeholder="请输入关键词" class="input-sm form-control"> <span class="input-group-btn">
						<button id="findBtn" type="button" class="btn btn-sm btn-primary"><i class="fa fa-search"> 搜索</i> </button> </span>
                    </div>
                    <div id="displayTable" th:fragment="displayTable">
                        <table class="footable table table-stripped toggle-arrow-tiny">
                            <thead>
                            <tr>
                                <th>编号</th>
                                <th>品牌名称</th>
                                <th>首字母</th>
                                <th>排序号</th>
                                <th>产品数量</th>
                                <th>产品评论数量</th>
                                <th>品牌故事</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr th:each="item: ${itemList}">
                                <td th:text="${item.id}"></td>
                                <td th:text="${item.name}"></td>
                                <td th:text="${item.firstLetter}"></td>
                                <td th:text="${item.sort}"></td>
                                <td th:text="${item.productCount}"></td>
                                <td th:text="${item.productCommentCount}"></td>
                                <td th:text="${item.brandStory}"></td>
                                <td style="width: 50px;">
                                    <button th:onclick="updateBtn([[${item.id}]],[[${item.name}]],[[${item.firstLetter}]],[[${item.sort}]],[[${item.productCount}]],[[${item.productCommentCount}]],[[item.brandStory]])"
                                            class="updateUserBtn btn btn-warning btn-xs btn-rounded" type="button">
                                        <i class="fa fa-edit"></i> 修改
                                    </button>
                                </td>
                                <td style="width: 50px;">
                                    <button th:onclick="|deleteBtn(${item.id})|" class="deleteUserBtn btn btn-danger btn-xs btn-rounded" type="button">
                                        <i class="fa fa-trash"></i> 删除
                                    </button>
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- 添加角色的弹出框 -->
<div id="modal-form-add" class="modal fade" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-body">
                <div class="row">
                    <div class="col-sm-12 b-r">
                        <div class="ibox-title">
                            <h5>添加新的品牌</h5>
                        </div>
                        <div class="ibox-content">
                            <form id="addForm" class="form-horizontal">
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">品牌：</label>
                                    <div class="col-sm-8">
                                        <input id="addName" type="text" placeholder="品牌名" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">品牌首字母：</label>
                                    <div class="col-sm-8">
                                        <input id="addFirstLetter" type="text" placeholder="品牌首字母" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">排序号：</label>
                                    <div class="col-sm-8">
                                        <input id="addSort" type="text" placeholder="排序号" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">产品数量：</label>
                                    <div class="col-sm-8">
                                        <input id="addProductCount" type="text" placeholder="产品数量" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">产品评论数量：</label>
                                    <div class="col-sm-8">
                                        <input id="addProductCommentCount" type="text" placeholder="产品数量" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">品牌故事：</label>
                                    <div class="col-sm-8">
                                        <input id="addBrandStory" type="text" placeholder="品牌故事" class="form-control">
                                    </div>
                                </div>

                                <div>
                                    <button id="addSubmitBtn" class="btn btn-primary btn-primary pull-right m-t-n-xs" type="button"><i class="fa fa-check"></i>&nbsp;提交
                                    </button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
                                        
<!-- 修改角色的弹出框 -->
<div id="modal-form-update" class="modal fade" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-body">
                <div class="row">
                    <div class="col-sm-12 b-r">
                        <div class="ibox-title">
                            <h5>修改用户信息</h5>
                        </div>
                        <div class="ibox-content">
                            <form id="updateForm" class="form-horizontal">
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">编号：</label>
                                    <div class="col-sm-8">
                                        <input id="updateBrandId" type="text" placeholder="编号" class="form-control" disabled>
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">品牌：</label>
                                    <div class="col-sm-8">
                                        <input id="updateName" type="text" placeholder="品牌名" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">品牌首字母：</label>
                                    <div class="col-sm-8">
                                        <input id="updateFirstLetter" type="text" placeholder="品牌首字母" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">排序号：</label>
                                    <div class="col-sm-8">
                                        <input id="updateSort" type="text" placeholder="排序号" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">产品数量：</label>
                                    <div class="col-sm-8">
                                        <input id="updateProductCount" type="text" placeholder="产品数量" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">产品评论数量：</label>
                                    <div class="col-sm-8">
                                        <input id="updateProductCommentCount" type="text" placeholder="产品数量" class="form-control">
                                    </div>
                                </div>
                                <div class="form-group">
                                    <label class="col-sm-3 control-label">品牌故事：</label>
                                    <div class="col-sm-8">
                                        <input id="updateBrandStory" type="text" placeholder="品牌故事" class="form-control">
                                    </div>
                                </div>

                                <div>
                                    <button id="updateSubmitBtn" class="btn btn-primary btn-primary pull-right m-t-n-xs" type="button"><i class="fa fa-check"></i>&nbsp;提交
                                    </button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script src="/static/js/jquery.min.js?v=2.1.4"></script>
<script src="/static/js/bootstrap.min.js?v=3.3.6"></script>
<script src="/static/js/plugins/peity/jquery.peity.min.js"></script>
<script src="/static/js/content.min.js?v=1.0.0"></script>
<script src="/static/js/plugins/iCheck/icheck.min.js"></script>
<script src="/static/js/demo/peity-demo.min.js"></script>
<script src="/static/js/myJS/brand.js"></script>

<script th:inline="javascript">
    function deleteBtn(id) {
        if (confirm("确定要删除吗？")) {
            $.ajax({
                type: 'GET',
                url: '/brand/delete/'+id,
                success: function (data) {
                    $('#displayTable').html(data)
                },
                error: function (err) {
                    console.log(err);
                    alert("操作失败，请刷新重新尝试！")
                }
            })
        }

    }

    // 点击修改按钮
    function updateBtn(id,name,firstLetter,sort,productCount,productCommentCount,brandStory) {
        // 传递数据到弹出框
        $('#modal-form-update').modal('show');
        $('#updateBrandId').val(id);
        $('#updateName').val(name);
        $('#updateFirstLetter').val(firstLetter);
        $('#updateSort').val(sort);
        $('#updateProductCount').val(productCount);
        $('#updateProductCommentCount').val(productCommentCount);
        $('#updateBrandStory').val(brandStory);
    }
</script>
</body>
</html>

```

# 2、Business Handle

```javascript
//监听添加按钮事件
$('#addBtn').click(function () {
    //添加按钮被点击后，展示model框
    $('#modal-form-add').modal('show');
});

//查询
$('#findBtn').click(function () {
    $.ajax({
            type:'GET',
            url:'/brand/select',
            data:{'search':$('#search').val()},
            success:function (data) {
                $('#displayTable').html(data);
            },
            error: function (err) {
                console.log(err);
                alert('操作失败，请刷新重新尝试！');
            }
        }
    )
});
//添加
$('#addSubmitBtn').click(function () {
    var name = $('#addName').val();
    var firstLetter = $('#addFirstLetter').val();
    var sort = $('#addSort').val();
    var productCount = $('#addProductCount').val();
    var productCommentCount = $('#addProductCommentCount').val();
    var brandStory = $('#addBrandStory').val();

    //非空校验
    if (name.length === 0){
        alert('品牌名称不能为空');
    }else if (firstLetter.length === 0){
        alert('品牌首字母不能为空');
    }else if (sort.length === 0){
        alert('排序号不能为空');
    }else if (productCount.length === 0){
        alert('产品数量不能为空');
    }else if (productCommentCount.length === 0){
        alert('产品评论数量不能为空');
    } else if (brandStory.length === 0){
        alert('品牌故事不能为空');
    } else {
        $.ajax({
                type:'POST',
                url:'/brand/insert',
                data:{
                    'name':name,
                    'firstLetter':firstLetter,
                    'sort':sort,
                    'productCount':productCount,
                    'productCommentCount':productCommentCount,
                    'brandStory':brandStory
                },
                success: function (data) {
                    //关闭模型框
                    $('#modal-form-add').modal('hide');
                    
                    //清空模型库里上一次的数据
                    document.getElementById('addForm').reset();
                    
                    //局部刷新
                    $('#displayTable').html(data);
                },
                
              error: function (err) {
                    console.log(err);
                    alert('操作失败，请刷新重新尝试！');
                }

            }


        );


    }
});
//修改提交
$('#updateSubmitBtn').click(function () {
    var id = $('#updateBrandId').val();
    var name = $('#updateName').val();
    var firstLetter = $('#updateFirstLetter').val();
    var sort = $('#updateSort').val();
    var productCount = $('#updateProductCount').val();
    var productCommentCount = $('#updateProductCommentCount').val();
    var brandStory = $('#updateBrandStory').val();

    if (name.length === 0){
        alert('品牌名称不能为空');
    }else if (firstLetter.length === 0){
        alert('品牌首字母不能为空');
    }else if (sort.length === 0){
        alert('排序号不能为空');
    }else if (productCount.length === 0){
        alert('产品数量不能为空');
    }else if (productCommentCount.length === 0){
        alert('产品评论数量不能为空');
    } else if (brandStory.length === 0){
        alert('品牌故事不能为空');
    }else {
        $.ajax({
                type:'POST',
                url:'/brand/update',
                data:{
                    'id':id,
                    'name':name,
                    'firstLetter':firstLetter,
                    'sort':sort,
                    'productCount':productCount,
                    'productCommentCount':productCommentCount,
                    'brandStory':brandStory
                },
                success: function (data) {
                    //关闭模型框
                    $('#modal-form-update').modal('hide');
                    //清空模型库里上一次的数据
                    document.getElementById('updateForm').reset();
                    //局部刷新
                    $('#displayTable').html(data);
                },
                error: function (err) {
                    console.log(err);
                    alert('操作失败，请刷新重新尝试！');
                }

            }


        );


    }

});
```

