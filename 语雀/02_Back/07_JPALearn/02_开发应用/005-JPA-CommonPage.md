# JPA-CommonPage

> 因为本人角色，使用SpringBoot的分页信息对象直接返回前端不合理，其理由在于有太多不必要的参数了，我通常会定义一个分页交互模型，把需要的数据抽离，从Page抽离出来，再给到前端。

# 1、CommonPage

```java
@Data
public class CommonPage<T> {
    private Integer pageNum;
    private Integer pageSize;
    private Integer totalPage;
    private Long total;
    private List<T> list;


    /**
     * 将SpringData分页后的list转为分页信息
     */
    public static <T> CommonPage<T> restPage(Page<T> pageInfo) {
        CommonPage<T> result = new CommonPage<T>();
        result.setTotalPage(pageInfo.getTotalPages());
        result.setPageNum(pageInfo.getNumber());
        result.setPageSize(pageInfo.getSize());
        result.setTotal(pageInfo.getTotalElements());
        result.setList(pageInfo.getContent());
        return result;
    }


}

```

# 2、In Your Business

```java
    @RequestMapping("/findByPage")
    public String findByPage(
            @RequestParam(name = "pageNo",defaultValue = "0") Integer pageNo,
            @RequestParam(name = "pageSize",defaultValue = "5") Integer pageSize, Model model){
        log.info("==系统查询学生==");
       
        //分页查询
        List<Student> studentList = new ArrayList<>();
       
        Pageable pageable = PageRequest.of(pageNo, pageSize);
        
        CommonPage<Student> page = CommonPage.restPage(studentDao.findAll(pageable));
       
        //结果存入请求域
        model.addAttribute("page",page);
      
        //结果存入请求域
        model.addAttribute("studentList",studentList);

        //转发的学生list页面
        return "/student/student_list";
    }
```

```java
public interface StudentDao extends CrudRepository<Student,Long> {

    Page<Student> findAll(Pageable pageable);
}

```

