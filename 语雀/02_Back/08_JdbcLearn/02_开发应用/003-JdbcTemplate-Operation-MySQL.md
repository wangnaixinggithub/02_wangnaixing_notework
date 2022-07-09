#  JdbcTemplate-Operation-MySQL

```xml
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.9.RELEASE</version>
        </dependency>   
```

# 1、DataSourceUtils

```java
public class DataSourceUtils {

	private static Properties properties = new Properties();
	private static DruidDataSource dataSource;



	//1.通过类加载器获取Resource目录的数据库参数配置
	static{
		try {
			properties.load(DataSourceUtils.class.getClassLoader().getResourceAsStream("jdbc.properties"));
		} catch (IOException e) {
			e.printStackTrace();
		}
	}


	/**
	 * 获取数据库连接池
	 * @return 德鲁伊数据源
	 */
	@SneakyThrows
	public static DataSource getDateSource() {
		if (dataSource != null){ return dataSource; }
		//2.构造数据库连接池
		return DruidDataSourceFactory.createDataSource(properties);
	}

}

```

# 2、Your Business

```java
public interface PmsBrandMapper {
    /***
     * 查询全部品牌
     * @return 品牌集合
     */
    List<PmsBrand> findAll();

    /**
     * 新增品牌信息
     * @param pmsBrand 品牌模型
     */
    void insert(PmsBrand pmsBrand);

    /**
     * 根据ID删除品牌信息
     * @param id ID
     */
    void deleteById(Long id);

    /**
     * 更新品牌信息
     * @param pmsBrand 品牌模型
     */
    void update(PmsBrand pmsBrand);


    /**
     * 根据ID查询品牌信息
     * @return 品牌对象
     */
    PmsBrand findById(Long id);

    /**
     * 查询总记录
     * @return 表总记录数
     */
    long count();

    /**
     *分页查询
     * @param pageNo 当前页
     * @param pageSize 每页大小
     * @return 品牌集合
     */
    List<PmsBrand> findByPage(Integer pageNo, Integer pageSize);


    /**
     * 根据关键字查询总记录
     * @param keyword 查询关键字
     * @return 表总记录数
     */
    long count(String keyword);

    /**
     * 根据关键字分页查询
     * @param pageNo 当前页
     * @param pageSize 每页大小
     * @param keyword 查询关键字
     * @return 品牌集合
     */
    List<PmsBrand> findByPage(Integer pageNo, int pageSize, String keyword);
}

```

```java
public class PmsBrandMapperImpl implements PmsBrandMapper {

    private JdbcTemplate template = new JdbcTemplate(DataSourceUtils.getDateSource());


    public List<PmsBrand> findAll() {
    String sql = " SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory FROM pms_brand";
     return template.query(sql,new BeanPropertyRowMapper<>(PmsBrand.class));
    }

    public void insert(PmsBrand pmsBrand) {
        String sql = "INSERT INTO pms_brand ( name, first_letter, sort, factory_status, show_status, product_count, product_comment_count, logo, big_pic, brand_story )\n" +
                "        VALUES ( ?,  ?,  ?,  ?,  ?,  ?,  ?,  ?, ?, ?)";

            template.update(sql,
                    pmsBrand.getName(),
                    pmsBrand.getFirstLetter(),
                    pmsBrand.getSort(),
                    pmsBrand.getFactoryStatus(),
                    pmsBrand.getShowStatus(),
                    pmsBrand.getProductCount(),
                    pmsBrand.getProductCommentCount(),
                    pmsBrand.getLogo(),
                    pmsBrand.getBigPic(),
                    pmsBrand.getBrandStory());


    }

    public void deleteById(Long id) {
        String sql = " DELETE FROM pms_brand WHERE id= ?";
        template.update(sql,id);


    }

    public void update(PmsBrand pmsBrand) {
    String sql = "  UPDATE pms_brand SET\n" +
            "                             name= ?,\n" +
            "                             first_letter = ?,\n" +
            "                             sort = ?,\n" +
            "                             factory_status = ?,\n" +
            "                             show_status = ? ,\n" +
            "                             product_count = ? ,\n" +
            "                             product_comment_count = ?,\n" +
            "                             logo = ?,\n" +
            "                             big_pic= ?,\n" +
            "                             brand_story= ?\n" +
            "                             WHERE id= ? ";

             template.update(sql,
                    pmsBrand.getName(),
                    pmsBrand.getFirstLetter(),
                    pmsBrand.getSort(),
                    pmsBrand.getFactoryStatus(),
                    pmsBrand.getShowStatus(),
                    pmsBrand.getProductCount(),
                    pmsBrand.getProductCommentCount(),
                    pmsBrand.getLogo(),
                    pmsBrand.getBigPic(),
                    pmsBrand.getBrandStory(),
                    pmsBrand.getId());

    }

    public PmsBrand findById(Long id) {
        String sql = " SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand WHERE id= ?";
        return template.queryForObject(sql,new BeanPropertyRowMapper<>(PmsBrand.class),id);

    }

    public long count() {
        String sql = "SELECT COUNT(1) FROM pms_brand";
        return template.queryForObject(sql, Long.class);

    }

    public List<PmsBrand> findByPage(Integer pageNo, Integer pageSize) {
        Integer index = (pageNo - 1) * pageSize;
        String sql = "SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand LIMIT ?,?";
        return template.query(sql,new BeanPropertyRowMapper<>(PmsBrand.class),index,pageSize);

    }

    @Override
    public long count(String keyword) {
        keyword = "%" + keyword + "%";
        String sql = "SELECT COUNT(1) FROM pms_brand where name like ?";
        return  template.queryForObject(sql,Long.class,keyword);
    }

    @Override
    public List<PmsBrand> findByPage(Integer pageNo, int pageSize, String keyword) {
        keyword = "%" + keyword + "%";
        Integer index = (pageNo - 1) * pageSize;
        String sql = "SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand where name like  ? LIMIT ?,? ";
        return template.query(sql,new BeanPropertyRowMapper<>(PmsBrand.class),keyword,index,pageSize);
    }
}

```



