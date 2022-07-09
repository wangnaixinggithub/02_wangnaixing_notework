# SpringBoot-Merge-Ehcache

# 1、Need Config

```xml
      <!--ehcache缓存-->
        <dependency>
            <groupId>net.sf.ehcache</groupId>
            <artifactId>ehcache</artifactId>
        </dependency>
```

```yaml
 spring:
    cache:
       ehcache:
         config: classpath*:ehcache.xml
```

```java

@SpringBootApplication
@EnableCaching
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

```xml
<!--ehcache.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="ehcache.xsd">
    
    <defaultCache
            maxElementsInMemory="10000"
            eternal="false"
            timeToIdleSeconds="3600"
            timeToLiveSeconds="0"
            overflowToDisk="false"
            diskPersistent="false"
            diskExpiryThreadIntervalSeconds="120" />

    <cache
            name="student"
            maxEntriesLocalHeap="2000"
            eternal="false"
            timeToIdleSeconds="3600"
            timeToLiveSeconds="0"
            overflowToDisk="false"
            statistics="true"/>
    
</ehcache>
```

# 2、Your Business

```java
@Service
public class PmsBrandServiceImpl implements PmsBrandService {
    @Resource
    private PmsBrandMapper brandMapper;


    @Override
    public List<PmsBrand> findAll() {
        return brandMapper.selectByExample(null);
    }

    @Override
    public CommonPage<PmsBrand> findByPage(Integer pageNum, Integer pageSize) {
        PageHelper.startPage(pageNum,pageSize);
        List<PmsBrand> brandList = brandMapper.selectByExample(null);
        return CommonPage.restPage(brandList);
    }

    @Override
    public void save(PmsBrand pmsBrand) {
        brandMapper.insert(pmsBrand);
    }

    @Override
    @CacheEvict(value = "mall",key = "'pms:brand:'+#id")
    public void delete(Long id) {
        brandMapper.deleteByPrimaryKey(id);
    }

    @Override
    @CacheEvict(value = "mall",key = "'pms:brand:'+#id")
    public void update(PmsBrand pmsBrand) {
        brandMapper.updateByPrimaryKey(pmsBrand);
    }

    @Override
    @Cacheable(value = "mall",key = "'pms:brand:'+#id",unless = "#result==null")
    public PmsBrand findById(Long id) {
        return brandMapper.selectByPrimaryKey(id);
    }
    
}

```

