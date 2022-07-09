# QueryRunner-Operation-MySQL

```xml
        <!--DBUtils-->
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.7</version>
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
    List<PmsBrand> findByPage( Integer pageNo,  Integer pageSize);
}

```

```java
public class PmsBrandMapperImpl implements PmsBrandMapper {

    private QueryRunner queryRunner = new QueryRunner(DataSourceUtils.getDateSource());
    /***
     * 查询全部品牌
     * @return 品牌集合
     */
    public List<PmsBrand> findAll() {
    String sql = " SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory FROM pms_brand";

        try {
            return queryRunner.query(sql,new BeanListHandler<PmsBrand>(PmsBrand.class));
        } catch (SQLException e) {
            e.printStackTrace();
            return null;
        }
    }

    public void insert(PmsBrand pmsBrand) {
        String sql = "INSERT INTO pms_brand ( name, first_letter, sort, factory_status, show_status, product_count, product_comment_count, logo, big_pic, brand_story )\n" +
                "        VALUES ( ?,  ?,  ?,  ?,  ?,  ?,  ?,  ?, ?, ?} )";

        try {

            queryRunner.update(sql,
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

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public void deleteById(Long id) {
        String sql = " DELETE FROM pms_brand WHERE id= #{id}";
        try {
            queryRunner.update(sql,id);
        } catch (SQLException e) {
            e.printStackTrace();
        }

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


        try {

             queryRunner.update(sql,
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

        } catch (SQLException e) {
            e.printStackTrace();
        }

    }

    public PmsBrand findById(Long id) {
        String sql = " SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand WHERE id= ?";

        try {
            return queryRunner.query(sql,new BeanHandler<PmsBrand>(PmsBrand.class),id);
        } catch (SQLException e) {
            e.printStackTrace();
            return null;
        }
    }

    public long count() {
        String sql = "SELECT COUNT(1) FROM pms_brand";
        try {
            return queryRunner.query(sql,new ScalarHandler<Long>());
        } catch (SQLException e) {
            e.printStackTrace();
            return 0L;
        }

    }

    public List<PmsBrand> findByPage(Integer pageNo, Integer pageSize) {
        Integer index = (pageNo - 1) * pageSize;
        String sql = "SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand LIMIT ?,?";

        try {
            return queryRunner.query(sql,new BeanListHandler<PmsBrand>(PmsBrand.class),index,pageSize);
        } catch (SQLException e) {
            e.printStackTrace();
            return null;
        }
    }
}

```

