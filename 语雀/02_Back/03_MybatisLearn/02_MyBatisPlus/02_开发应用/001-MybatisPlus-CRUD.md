# MyBatisPlus-CRUD

## 1、BaseMapper

```java
public interface BaseMapper<T> extends Mapper<T> {

    /**
     * 插入一条记录
     *
     * @param entity 实体对象
     */
    int insert(T entity);

    /**
     * 根据 ID 删除
     *
     * @param id 主键ID
     */
    int deleteById(Serializable id);

    /**
     * 根据实体(ID)删除
     *
     * @param entity 实体对象
     * @since 3.4.4
     */
    int deleteById(T entity);

    /**
     * 根据 columnMap 条件，删除记录
     *
     * @param columnMap 表字段 map 对象
     */
    int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

    /**
     * 根据 entity 条件，删除记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
     */
    int delete(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 删除（根据ID 批量删除）
     *
     * @param idList 主键ID列表(不能为 null 以及 empty)
     */
    int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);

    /**
     * 根据 ID 修改
     *
     * @param entity 实体对象
     */
    int updateById(@Param(Constants.ENTITY) T entity);

    /**
     * 根据 whereEntity 条件，更新记录
     *
     * @param entity        实体对象 (set 条件值,可以为 null)
     * @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）
     */
    int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);

    /**
     * 根据 ID 查询
     *
     * @param id 主键ID
     */
    T selectById(Serializable id);

    /**
     * 查询（根据ID 批量查询）
     *
     * @param idList 主键ID列表(不能为 null 以及 empty)
     */
    List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);

    /**
     * 查询（根据 columnMap 条件）
     *
     * @param columnMap 表字段 map 对象
     */
    List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

    /**
     * 根据 entity 条件，查询一条记录
     * <p>查询一条记录，例如 qw.last("limit 1") 限制取一条记录, 注意：多条数据会报异常</p>
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    default T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper) {
        List<T> ts = this.selectList(queryWrapper);
        if (CollectionUtils.isNotEmpty(ts)) {
            if (ts.size() != 1) {
                throw ExceptionUtils.mpe("One record is expected, but the query result is multiple records");
            }
            return ts.get(0);
        }
        return null;
    }

    /**
     * 根据 Wrapper 条件，查询总记录数
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    Long selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 entity 条件，查询全部记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录
     * <p>注意： 只返回第一个字段的值</p>
     *
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 entity 条件，查询全部记录（并翻页）
     *
     * @param page         分页查询条件（可以为 RowBounds.DEFAULT）
     * @param queryWrapper 实体对象封装操作类（可以为 null）
     */
    <P extends IPage<T>> P selectPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

    /**
     * 根据 Wrapper 条件，查询全部记录（并翻页）
     *
     * @param page         分页查询条件
     * @param queryWrapper 实体对象封装操作类
     */
    <P extends IPage<Map<String, Object>>> P selectMapsPage(P page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
}

```

## 2、API Test

```java
@SpringJUnitConfig(classes = {SpringConfig.class})
public class MybatisPlusTest {
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testInsert(){
        User user = new User(null, "张三", 23, "zhangsan@atguigu.com");
        int result = userMapper.insert(user);
        //INSERT INTO user ( id, name, age, email ) VALUES ( ?, ?, ?, ? )
        System.out.println("受影响行数："+result);
    //1502460402466648066  Mybatis-plus 默认使用雪花算法生成ID
        System.out.println("id自动获取："+user.getId());
    }

    @Test
    public void testDeleteById(){
//通过id删除用户信息
//DELETE FROM user WHERE id=?
        int result = userMapper.deleteById(1502460402466648066L);
        System.out.println("受影响行数："+result);
    }

    @Test
    public void testDeleteBatchIds(){
//通过多个id批量删除
//DELETE FROM user WHERE id IN ( ? , ? , ? )
        List<Long> idList = Arrays.asList(1L, 2L, 3L);
        int result = userMapper.deleteBatchIds(idList);
        System.out.println("受影响行数："+result);
    }


    @Test
    public void testDeleteByMap(){
//根据map集合中所设置的条件删除记录
//DELETE FROM user WHERE name = ? AND age = ?
        Map<String, Object> map = new HashMap<>();
        map.put("age", 23);
        map.put("name", "张三");
        int result = userMapper.deleteByMap(map);
        System.out.println("受影响行数："+result);
    }

    @Test
    public void testUpdateById(){
        User user = new User(4L, "admin", 22, null);
//UPDATE user SET name=?, age=? WHERE id=?
        int result = userMapper.updateById(user);
        System.out.println("受影响行数："+result);
    }

    @Test
    public void testSelectById(){
//根据id查询用户信息
//SELECT id,name,age,email FROM user WHERE id=?
        User user = userMapper.selectById(4L);
        System.out.println(user);
    }

    @Test
    public void testSelectBatchIds(){
//根据多个id查询多个用户信息
//SELECT id,name,age,email FROM user WHERE id IN ( ? , ? )
        List<Long> idList = Arrays.asList(4L, 5L);
        List<User> list = userMapper.selectBatchIds(idList);
        list.forEach(System.out::println);
    }

    @Test
    public void testSelectByMap(){
//通过map条件查询用户信息
//SELECT id,name,age,email FROM user WHERE name = ? AND age = ?
        Map<String, Object> map = new HashMap<>();
        map.put("age", 22);
        map.put("name", "admin");
        List<User> list = userMapper.selectByMap(map);
        list.forEach(System.out::println);
    }

    @Test
    public void testSelectList(){
//查询所有用户信息
//SELECT id,name,age,email FROM user
        List<User> list = userMapper.selectList(null);
        list.forEach(System.out::println);
    }

}

```

## 3、CRUD

```java
@SpringBootTest
public class CrudTest {
    @Resource
    private UserMapper mapper;
    @Resource
    private User2Mapper user2Mapper;

    @Test
    public void aInsert(){
        User user = new User();
        user.setName("小羊");
        user.setAge(3);
        user.setEmail("abc@mp.com");
        int count = mapper.insert(user);
        assertThat(count).isGreaterThan(0);
        
        //成功直接拿到回写的ID
        assertThat(user.getId()).isNotNull();
    }

    @Test
    public void bDelete(){
        int count1 = mapper.deleteById(3L);
        int count2 = mapper.delete(new QueryWrapper<User>().lambda().eq(User::getName, "Sandy"));
        assertThat(count1).isGreaterThan(0);
        assertThat(count2).isGreaterThan(0);
    }
    @Test
    public void cUpdate(){

        //根据ID更新
        User user = new User();
        user.setId(1L);
        user.setEmail("ab@c.c");
        int count1 = mapper.updateById(user);

       //根据ID，更新name,更新email字段
        User user1 = new User();
        user1.setName("mp");
        int count2 = mapper.update(user1, Wrappers.<User>lambdaUpdate()
                .set(User::getEmail, "wangnaixing@qq.com")
                .eq(User::getId, 2));

        //根据ID更新email字段
        User user2 = new User();
        user2.setEmail("miemie@baomidou.com");
        int count3 = mapper.update(user2, new QueryWrapper<User>().lambda()
                .eq(User::getId, 2));

        //根据ID更新email字段方法2
        int count4 = mapper.update(null, Wrappers.<User>lambdaUpdate()
                .set(User::getEmail, "wnx@qq.com")
                .eq(User::getId, 2));

        //根据ID更新email,age字段
        User user3 = new User();
        user3.setEmail("miemie2@baomidou.com");
        int count5 = mapper.update(user3, Wrappers.<User>lambdaUpdate()
                .set(User::getAge, 20).eq(User::getId, 2L));

        assertThat(count1).isGreaterThan(0);
        assertThat(count2).isGreaterThan(0);
        assertThat(count3).isGreaterThan(0);
        assertThat(count4).isGreaterThan(0);
        assertThat(count5).isGreaterThan(0);


    }

    @Test
    public void dSelect() {
        mapper.insert(
                new User().setId(10086L).setName("miemie")
                        .setEmail("miemie@baomidou.com")
                        .setAge(3)
        );
        //根据ID查询
        User user = mapper.selectById(10086L);

        //根据条件查询
        User user1 = mapper.selectOne(new QueryWrapper<User>().lambda().eq(User::getId, 10086L));

        //查询全部ID构成的集合
        List<User> userList = mapper.selectList(Wrappers.<User>lambdaQuery().select(User::getId));
        userList.forEach(System.out::println);

       //查询id,name字段构成的集合
        List<User> userList1 = mapper.selectList(new QueryWrapper<User>().select("id", "name"));
        //查询id,name字段构成的集合 lambda方式
        List<User> userList2 = mapper.selectList(new QueryWrapper<User>().lambda()
                .select(User::getId, User::getName));
        userList1.forEach(System.out::println);
        userList2.forEach(System.out::println);

        //查询全部字段构成的集合
        List<User> selectList = mapper.selectList(null);
        selectList.forEach(System.out::println);
    }

    @Test
    public void orderBy() {
        //根据年龄排序
        List<User> userList1 = mapper.selectList(Wrappers.<User>query().orderByAsc("age"));
        //根据年龄和姓名升序
        List<User> userList3 = mapper.selectList(Wrappers.<User>query().orderByAsc("age", "name"));
        //先按照age升序排列，age相同再按照name降序排列
        List<User> userList5 = mapper.selectList(Wrappers.<User>query().orderByAsc("age").orderByDesc("name"));


        //根据年龄排序  lambda方式
        List<User> userList2 = mapper.selectList(Wrappers.<User>lambdaQuery().orderByAsc(User::getAge));
        //根据年龄和姓名升序 lambada方式
        List<User> userList4 = mapper.selectList(Wrappers.<User>lambdaQuery().orderByAsc(User::getAge, User::getName));
       //先按照age升序排列，age相同再按照name降序排列 lambada方式
        List<User> userList6 = mapper.selectList(Wrappers.<User>lambdaQuery().orderByAsc(User::getAge).orderByDesc(User::getName));

    }


    @Test
    public void selectMaps() {
        List<Map<String, Object>> mapList = mapper.selectMaps(Wrappers.<User>lambdaQuery().orderByAsc(User::getAge));
        assertThat(mapList).isNotEmpty();
        assertThat(mapList.get(0)).isNotEmpty();
        mapList.forEach(System.out::println);
    }

    @Test
    public void selectMapsPage(){
        Page<Map<String, Object>> page = mapper.selectMapsPage(new Page<>(1, 5),
                Wrappers.<User>lambdaQuery().orderByAsc(User::getAge));
        page.getRecords().forEach(System.out::println);
    }
    @Test
    public void testSelectMaxId() {
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.select("max(id) as id");
        User user = mapper.selectOne(wrapper);
        System.out.println("maxId="+user.getId());
    }

    @Test
    public void testGroup() {
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.select("age,count(*)").groupBy("age").orderByAsc("age");
        List<Map<String, Object>> mapList = mapper.selectMaps(wrapper);
        mapList.forEach(System.out::println);


        /**
         * lambdaQueryWrapper groupBy orderBy
         */
        LambdaQueryWrapper<User> lambdaQueryWrapper = new QueryWrapper<User>()
                .lambda()
                .select(User::getAge)
                .groupBy(User::getAge)
                .orderByAsc(User::getAge);
        List<User> userList = mapper.selectList(lambdaQueryWrapper);
        userList.forEach(System.out::println);

    }


    @Test
    public void testSqlCondition() {
        //根据模型属性进行 where = 查询
        List<User2> user2List = user2Mapper.selectList(Wrappers.<User2>query().setEntity(new User2().setName("Jone")));
        Assertions.assertEquals(user2List.size(),1);

        //name字段模糊查询
        List<User2> user2List1 = user2Mapper.selectList(Wrappers.<User2>query().like("name", "j"));
        Assertions.assertEquals(user2List1.size(),2);

        //年龄大于18 姓名有J
        List<User2> user2List2 = user2Mapper.selectList(Wrappers.<User2>query()
                .ge("age", 18).like("name","j"));
        Assertions.assertEquals(user2List.size(),1);

    }


}

```

