# JPA-CrudRepository-Method

```java
@NoRepositoryBean
public interface CrudRepository<T, ID extends Serializable> extends Repository<T, ID> {
    
    	//保存给定的实体。并返回数据库存好的，比如保存时ID没有值，数据库设置自增，返回的实体就改变了，ID就等于现在自增的ID
		<S extends T> S save(S entity);
       
    	//保存多个实体
		<S extends T> Iterable<S> save(Iterable<S> entities);
    	
    	//根据ID查询实体，没有返回空
		T findOne(ID id);
      
   		 //根据ID判断是否存在该实体
		boolean exists(ID id);
    	
   		 //查看全部实体
		Iterable<T> findAll();
    	
   		 //根据ID集合查询指定ID的实体集合
		Iterable<T> findAll(Iterable<ID> ids);
      
   		 //查询总数
		long count();
        
    	//根据ID删除实体
		void delete(ID id);
    	
   		 //根据实体删除实体
		void delete(T entity);
    	
    	//根据给定的实体集合删除实体集合
		void delete(Iterable<? extends T> entities);
    	
   		 //删除全部实体
		void deleteAll();
｝
```

