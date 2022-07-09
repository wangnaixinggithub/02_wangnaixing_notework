# SpringBoot-Merge-Redis

# 1、Your Config

```xml
      <!--redis依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
```

```yaml
spring:
  redis:
    host: 127.0.0.1 # Redis服务器地址
    database: 0 # Redis数据库索引（默认为0）
    port: 6379 # Redis服务器连接端口
    password: # Redis服务器连接密码（默认为空）
    jedis:
      pool:
        max-active: 8 # 连接池最大连接数（使用负值表示没有限制）
        max-wait: -1ms # 连接池最大阻塞等待时间（使用负值表示没有限制）
        max-idle: 8 # 连接池中的最大空闲连接
        min-idle: 0 # 连接池中的最小空闲连接
    timeout: 3000ms # 连接超时时间（毫秒）
```

```java
@Configuration
@EnableCaching
public class RedisConfig  extends CachingConfigurerSupport {
    
    //1、RedisTemplate
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisSerializer<Object> serializer = redisSerializer();
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
       
        redisTemplate.setConnectionFactory(redisConnectionFactory);
       
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(serializer);
       
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(serializer);
        redisTemplate.afterPropertiesSet();
        
        return redisTemplate;
    }
    
	//2、RedisSerializer
    @Bean
    public RedisSerializer<Object> redisSerializer() {
        //创建JSON序列化器
        Jackson2JsonRedisSerializer<Object> serializer = new Jackson2JsonRedisSerializer<>(Object.class);
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        serializer.setObjectMapper(objectMapper);
        return serializer;
    }
    
    //3、RedisCacheManager
    @Bean
    public RedisCacheManager redisCacheManager(RedisConnectionFactory redisConnectionFactory) {
        RedisCacheWriter redisCacheWriter = RedisCacheWriter.nonLockingRedisCacheWriter(redisConnectionFactory);
       
        //设置Redis缓存有效期为1天
        RedisCacheConfiguration redisCacheConfiguration = RedisCacheConfiguration.defaultCacheConfig()
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer())).entryTtl(Duration.ofDays(1));
        
        return new RedisCacheManager(redisCacheWriter, redisCacheConfiguration);
   
    }

    
}

```

# 2、Your Business

```java
/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/1/16 13:29
 */
public interface RedisService {
    /**
     * 保存属性
     * @param key
     * @param value
     * @param time
     */
    void set(String key,Object value,long time);

    /**
     * 保存属性
     * @param key
     * @param value
     */
    void set(String key,Object value);

    /**
     * 获取属性
     * @param key
     * @return
     */
    Object get(String key);

    /**
     * 删除属性
     * @param key
     * @return
     */
    Boolean del(String key);

    /**
     * 批量删除属性
     * @param keys
     * @return
     */
    Long del(List<String> keys);

    /**
     * 设置过期时间
     * @param key
     * @param time
     * @return
     */
    Boolean expire(String key,long time);

    /**
     * 判断是否有该属性
     * @param key
     * @return
     */
    Boolean hasKey(String key);

    /**
     * 按delta递增
     * @param key
     * @param delta
     * @return
     */
    Long incr(String key,long delta);

    /**
     * 按照delta值递减
     * @param key
     * @param delta
     * @return
     */
    public Long decr(String key, long delta);


    /**
     * 获取Hash结构中的属性
     * @param key
     * @param hashKey
     * @return
     */
    Object hGet(String key,String hashKey);

    /**
     * 向Hash结构中放入一个属性
     * @param key
     * @param hashKey
     * @param value
     * @param time
     * @return
     */
    Boolean hSet(String key,String hashKey,Object value,long time);

    /**
     * 直接设置整个Hash结构
     * @param key
     * @param map
     * @param time
     * @return
     */
    Boolean hSetAll(String key, Map<String,Object> map, long time);

    /**
     * 删除Hash结构中的属性
     * @param key
     * @param hashKey
     */
    void hDel(String key,Object...hashKey);


    /**
     * 判断Hash结构中是否有该属性
     * @param key
     * @param hashKey
     * @return
     */
    Boolean hHashKey(String key,String hashKey);

    /***
     * Hash结构中的属性递增
     * @param key
     * @param hashKey
     * @param delta
     * @return
     */
    Long hIncr(String key,String hashKey,Long delta);


    /**
     * Hash结构中属性递减
     * @param key
     * @param hashKey
     * @param delta
     * @return
     */
    Long hDecr(String key,String hashKey,Long delta);

    /**
     * 获取Set结构
     * @param key
     * @return
     */
    Set<Object> sMembers(String key);

    /**
     * 向Set结构中添加属性
     * @param key
     * @param value
     * @return
     */
    Long sAdd(String key,Object...value);

    /**
     * 是否为Set中属性
     * @param key
     * @param value
     * @return
     */
    Boolean sIsMember(String key,Object value);

    /**
     * 获取Set结构的长度
     * @param key
     * @return
     */
    Long sSize(String key);

    /**
     * 删除Set结构中的属性
     * @param key
     * @param values
     * @return
     */
    Long sRemove(String key,Object...values);


}

```

```java
@Service
public class RedisServiceImpl implements RedisService {
    
    @Resource
    private RedisTemplate<String,Object> redisTemplate;


    @Override
    public void set(String key, Object value, long time) {
        redisTemplate.opsForValue().set(key,value,time, TimeUnit.SECONDS);
    }

    @Override
    public void set(String key, Object value) {
        redisTemplate.opsForValue().set(key,value);

    }

    @Override
    public Object get(String key) {
        return redisTemplate.opsForValue().get(key);
    }

    @Override
    public Boolean del(String key) {
        return redisTemplate.delete(key);
    }

    @Override
    public Long del(List<String> keys) {
        return redisTemplate.delete(keys);
    }

    @Override
    public Boolean expire(String key, long time) {
        return redisTemplate.expire(key,time,TimeUnit.SECONDS);
    }

    @Override
    public Boolean hasKey(String key) {
        return redisTemplate.hasKey(key);
    }

    @Override
    public Long incr(String key, long delta) {
        return redisTemplate.opsForValue().increment(key,delta);
    }

    @Override
    public Long decr(String key, long delta) {
        return redisTemplate.opsForValue().increment(key,-delta);
    }

    @Override
    public Object hGet(String key, String hashKey) {
        return redisTemplate.opsForHash().get(key,hashKey);
    }

    @Override
    public Boolean hSet(String key, String hashKey, Object value, long time) {
        redisTemplate.opsForHash().put(key,hashKey,value);
        return expire(key,time);
    }

    @Override
    public Boolean hSetAll(String key, Map<String, Object> map, long time) {
        redisTemplate.opsForHash().putAll(key,map);
        return expire(key,time);
    }

    @Override
    public void hDel(String key, Object... hashKey) {
        redisTemplate.opsForHash().delete(key,hashKey);
    }

    @Override
    public Boolean hHashKey(String key, String hashKey) {
        return redisTemplate.opsForHash().hasKey(key,hashKey);
    }

    @Override
    public Long hIncr(String key, String hashKey, Long delta) {
        return redisTemplate.opsForHash().increment(key,hashKey,delta);
    }

    @Override
    public Long hDecr(String key, String hashKey, Long delta) {
        return redisTemplate.opsForHash().increment(key,hashKey,-delta);
    }

    @Override
    public Set<Object> sMembers(String key) {
        return redisTemplate.opsForSet().members(key);
    }

    @Override
    public Long sAdd(String key, Object... values) {
        return redisTemplate.opsForSet().add(key,values);
    }

    @Override
    public Boolean sIsMember(String key, Object value) {
        return redisTemplate.opsForSet().isMember(key,value);
    }

    @Override
    public Long sSize(String key) {
        return redisTemplate.opsForSet().size(key);
    }

    @Override
    public Long sRemove(String key, Object... values) {
        return redisTemplate.opsForSet().remove(key,values);
    }
}

```

# 3、@CacheEvict   @Cacheable

```java
@Service
public class PmsBrandServiceImpl implements PmsBrandService {

    @Autowired
    private PmsBrandMapper pmsBrandMapper;

    /**
     * 新增品牌
     * @param pmsBrand
     */
    @Override
    public void save(PmsBrand pmsBrand) {

        pmsBrandMapper.insert(pmsBrand);

    }
    
    /**
     * 删除品牌
     * @param id
     */
    @CacheEvict(value = "mall", key = "'pms:brand:'+#id")
    @Override
    public void delete(Long id) {
        
        pmsBrandMapper.deleteByPrimaryKey(id);

    }

    /**
     *修改品牌
     * @param pmsBrand
     * @param id
     */
    @Override
    @CacheEvict(value = "mall", key = "'pms:brand:'+#id")
    public void update(PmsBrand pmsBrand, Long id) {
        
        pmsBrand.setId(id);
        
        pmsBrandMapper.updateByPrimaryKey(pmsBrand);

    }

    /**
     * 根据ID查询品牌
     * @param id
     * @return
     */
    @Override
    @Cacheable(value = "mall", key = "'pms:brand:'+#id", unless = "#result==null")
    public PmsBrand findById(Long id) {
        
        return pmsBrandMapper.selectByPrimaryKey(id);

    }

    /**
     * 分页查询品牌
     * @param pageNum
     * @param pageSize
     * @return
     */
    @Override
    public CommonPage<PmsBrand> findByPage(Integer pageNum, Integer pageSize) {

        //使用分页插件开启分页
        PageHelper.startPage(pageNum,pageSize);
        
        //查询全部品牌
        List<PmsBrand> pmsBrandList = pmsBrandMapper.selectByExample(new PmsBrandExample());
     
        //封装数据成通用分页对象返回
        return CommonPage.restPage(pmsBrandList);
    }

    
    @Override
    public List<PmsBrand> findAll() {
        
      return pmsBrandMapper.selectByExample(new PmsBrandExample());
   
    }

}
```

