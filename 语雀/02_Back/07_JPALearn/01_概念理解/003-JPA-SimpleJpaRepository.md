# SimpleJpaRepository

# 1、SimpleJpaRepository

```java
@Repository
@Transactional(readOnly = true)
public class SimpleJpaRepository<T, ID extends Serializable>implements JpaRepository<T, ID>, JpaSpecificationExecutor<T> {
    
    private final EntityManager em;
    
}
```

# 2、EntityManager

```java
public interface EntityManager {
    	//持久化实体
		public void persist(Object entity);
    
    	//通过QSQL创建查询对象
        public Query createQuery(String qlString);
    
    	//删除实体
        public void remove(Object entity);
    
        //整合成当前持久化上下文的实体
    	public <T> T merge(T entity);
    
    	//判断实体是不是当前持久化上下文的实体
    	public boolean contains(Object entity);	
    
    	//根据实体类类型，和ID查询实体
        public <T> T find(Class<T> entityClass, Object primaryKey);
    
    	//根据实体类型，ID获取实体引用
        public <T> T getReference(Class<T> entityClass, Object primaryKey);
    
    	//获取标准构造器
    	public CriteriaBuilder getCriteriaBuilder();
    
    	//通过标准查询创建查询接口
        public <T> TypedQuery<T> createQuery(CriteriaQuery<T> criteriaQuery);
    
    
   
}
```

```java
public interface TypedQuery<X> extends Query {
    
	//把查询结果转换为类型化列表返回
	 List<X> getResultList();
｝
```

```java
public interface CriteriaBuilder {
    
    //创建具有指定结果类型的查询接口
	<T> CriteriaQuery<T> createQuery(Class<T> resultClass);
    
}
```

# 3、CRUD Implements

```java
public class SimpleJpaRepository<T, ID extends Serializable> implements JpaRepository<T, ID>, JpaSpecificationExecutor<T> {
	private static final String ID_MUST_NOT_BE_NULL = "The given id must not be null!";
	
    @Transactional
	public void delete(ID id) {
        //ID必须不为空，如果为空，抛出异常。
		Assert.notNull(id, ID_MUST_NOT_BE_NULL);
        
        //删除前查询一遍，如果为空了 抛出空结果访问异常。告知要删除的实体并不存在。
		T entity = findOne(id);
		if (entity == null) {
			throw new EmptyResultDataAccessException(
					String.format("No %s entity with id %s exists!", entityInformation.getJavaType(), id), 1);
		}
        
        //再调用本类的delete()方法执行删除
		delete(entity);
	} 
    @Transactional
	public void delete(T entity) {
        //又是断言判断，保障实体不能为空
		Assert.notNull(entity, "The entity must not be null!");
        
        // 三目运算对实体进行处理整合后，调用实体管理器的remove()方法移除实体。
		em.remove(em.contains(entity) ? entity : em.merge(entity));
	}
    
    @Transactional
	public void delete(Iterable<? extends T> entities) {
        //断言判断，保障集合不为空
		Assert.notNull(entities, "The given Iterable of entities not be null!");
       
        //遍历删除
		for (T entity : entities) {
			delete(entity);
		}
	}
    
    @Transactional
	public void deleteInBatch(Iterable<T> entities) {
        //保证集合不为空
		Assert.notNull(entities, "The given Iterable of entities not be null!");
		
        if (!entities.iterator().hasNext()) {return;}
  		
        //进入获取Query对，马上执行删除操作。
		applyAndBind(getQueryString(DELETE_ALL_QUERY_STRING, entityInformation.getEntityName()), entities, em)
				.executeUpdate();
	}
    
    public static <T> Query applyAndBind(String queryString, Iterable<T> entities, EntityManager entityManager) {
		//判断入参不为空
		Assert.notNull(queryString, "Querystring must not be null!");
		Assert.notNull(entities, "Iterable of entities must not be null!");
		Assert.notNull(entityManager, "EntityManager must not be null!");
       
        //一般的删除，就遍历一次返回一次Query对象
		Iterator<T> iterator = entities.iterator();
		if (!iterator.hasNext()) {
			return entityManager.createQuery(queryString);
		}
        
        //处理有条件的删除
		String alias = detectAlias(queryString);
		StringBuilder builder = new StringBuilder(queryString);
		builder.append(" where");
		int i = 0;
		while (iterator.hasNext()) {
			iterator.next();
			builder.append(String.format(" %s = ?%d", alias, ++i));
			if (iterator.hasNext()) {
				builder.append(" or");
			}
		}
		Query query = entityManager.createQuery(builder.toString());
		iterator = entities.iterator();
		i = 0;
		while (iterator.hasNext()) {
			query.setParameter(++i, iterator.next());
		}
		return query;
	} 
 
    @Transactional
	public void deleteAll() {
        //查询全部后遍历删除
		for (T element : findAll()) {
			delete(element);
		}
	}
    
    //直接通过一个SQL删除全部
    @Transactional
	public void deleteAllInBatch() {
		em.createQuery(getDeleteAllQueryString()).executeUpdate();
	}
｝
```

# 4、 Query Implements

```java
public class SimpleJpaRepository<T, ID extends Serializable> implements JpaRepository<T, ID>, JpaSpecificationExecutor<T> {

    public T findOne(ID id) {
        //ID入参不能为空
		Assert.notNull(id, ID_MUST_NOT_BE_NULL);
		
        //获取实体类类型
		Class<T> domainType = getDomainClass();
		if (metadata == null) {
           
             //使用实体管理器根据类型和ID查询实体
			return em.find(domainType, id);
		}
    
    	//对锁模式的处理
		LockModeType type = metadata.getLockModeType();
		Map<String, Object> hints = getQueryHints();
		
      return type == null ? em.find(domainType, id, hints) : em.find(domainType, id, type, hints);
	}
    
 	public T getOne(ID id) {
        //保证ID不能为空
		Assert.notNull(id, ID_MUST_NOT_BE_NULL);
      
        //根据实体管理器的通过类型和ID获取引用
		return em.getReference(getDomainClass(), id);
	}    
  //获取到实体的查询接口，调用获取结果集方法查询全部结果集。 
  public List<T> findAll() {return getQuery(null, (Sort) null).getResultList();}
    
  protected TypedQuery<T> getQuery(Specification<T> spec, Sort sort) {return getQuery(spec, getDomainClass(), sort);}
    
  public List<T> findAll(Iterable<ID> ids) {
        //如果入参ids为空或者没有元素，调用Collections工具类，发挥空集合。
		if (ids == null || !ids.iterator().hasNext()) {
			return Collections.emptyList();
		}
		
      if (entityInformation.hasCompositeId()) {
            //创建一个容器
			List<T> results = new ArrayList<T>();
			for (ID id : ids) {
                //调用根据ID查询实体方法，放到容器中返回。
				results.add(findOne(id));
			}
			return results;
		}
		
      ByIdsSpecification<T> specification = new ByIdsSpecification<T>(entityInformation);
      TypedQuery<T> query = getQuery(specification, (Sort) null);
      
	return query.setParameter(specification.parameter, ids).getResultList();
	} 
    
  public List<T> findAll(Sort sort) {
        //得到Query接口，传入排序字段后再获取结果集。
		return getQuery(null, sort).getResultList();
	}
    
  //最后都要得到分页对象！  
  public Page<T> findAll(Pageable pageable) {
      	
      //分页条件为空，就默认查询全部，构造一个Page接口的实现PageImpl返回。
		if (null == pageable) {
            //相当于把查询全部的结果封装到了Page对象中。
			return new PageImpl<T>(findAll());
		}
    	
      //如果分页请求参数存就调用findAll()方法
		return findAll((Specification<T>) null, pageable);
	}
    
  public Page<T> findAll(Specification<T> spec, Pageable pageable) {
      	//得到查询接口对象
		TypedQuery<T> query = getQuery(spec, pageable);
      
      //调用读取分页的方法，返回Page对象。
		return pageable == null ? new PageImpl<T>(query.getResultList())
				: readPage(query, getDomainClass(), pageable, spec);
	}
    
  protected <S extends T> Page<S> readPage(TypedQuery<S> query, final Class<S> domainClass, Pageable pageable,
			final Specification<S> spec) {
      	
      //设置Query接口的index，即所谓的偏移量。
		query.setFirstResult(pageable.getOffset());
     
      //每页大小
		query.setMaxResults(pageable.getPageSize());
      
      //调用分页执行工具类的获取分页对象的方法返回分页对象。创建了总数供应商接口，并重写了get()
	return PageableExecutionUtils.getPage(query.getResultList(), pageable, new TotalSupplier() {
			@Override
			public long get() {
                
				return executeCountQuery(getCountQuery(spec, domainClass));
                
			}
		});
	}
   
    //查询全部，排序的方法
 	public List<T> findAll(Specification<T> spec, Sort sort) {
      
        //得到查询接口，获取结果集
		return getQuery(spec, sort).getResultList();
	}
    
    //通过事件进行查询单条
    public <S extends T> S findOne(Example<S> example) {｝
   
        //查询数量的方法   
    public <S extends T> long count(Example<S> example) {｝
    
      
}
    
```

```java
public abstract class PageableExecutionUtils {
	//提供特定查询总计数
    public interface TotalSupplier {
		long get();
	}
}
```

```java
protected <S extends T> TypedQuery<Long> getCountQuery(Specification<S> spec, Class<S> domainClass) {
		//通过实体管理器获取标准构造器
		CriteriaBuilder builder = em.getCriteriaBuilder();
       
        //通过标准构造器返回查询结果是Long类型的数据
		CriteriaQuery<Long> query = builder.createQuery(Long.class);

		Root<S> root = applySpecificationToCriteria(spec, domainClass, query);

		if (query.isDistinct()) {
			query.select(builder.countDistinct(root));
		} else {
			query.select(builder.count(root));
		}

		// Remove all Orders the Specifications might have applied
		query.orderBy(Collections.<Order> emptyList());
		
        //返回查询接口
		return em.createQuery(query);
	}

    private static Long executeCountQuery(TypedQuery<Long> query) {
            Assert.notNull(query, "TypedQuery must not be null!");
           //调用查询接口得到每一条记录数量
            List<Long> totals = query.getResultList();
            Long total = 0L;
            //for循环做++
            for (Long element : totals) {
                total += element == null ? 0 : element;
            }
            return total;
        }
}
```

`PageableExecutionUtils#getPage()`  do new `PageImpl`

```java
public abstract class PageableExecutionUtils {
		public static <T> Page<T> getPage(List<T> content, Pageable pageable, TotalSupplier totalSupplier) {
		Assert.notNull(content, "Content must not be null!");
		Assert.notNull(totalSupplier, "TotalSupplier must not be null!");
	
		if (pageable == null || pageable.getOffset() == 0) {
			if (pageable == null || pageable.getPageSize() > content.size()) {
				return new PageImpl<T>(content, pageable, content.size());
			}
 
			return new PageImpl<T>(content, pageable, totalSupplier.get());
		}
            

		if (content.size() != 0 && pageable.getPageSize() > content.size()) {
			return new PageImpl<T>(content, pageable, pageable.getOffset() + content.size());
		}
            

		return new PageImpl<T>(content, pageable, totalSupplier.get());
        }

}
```

# 5、PageImpl

```javascript
public class PageImpl<T> extends Chunk<T> implements Page<T> {
	private final long total;
	private final Pageable pageable;
  
  public PageImpl(List<T> content, Pageable pageable, long total) {
		super(content, pageable);
		this.pageable = pageable;
		this.total = !content.isEmpty() && pageable != null && pageable.getOffset() + pageable.getPageSize() > total
				? pageable.getOffset() + content.size() : total;
	}
  
  public PageImpl(List<T> content) {
		this(content, null, null == content ? 0 : content.size());
	}
}
```

```java
// Page接口扩展了获取总页码和获取总记录数的方法。
public interface Page<T> extends Slice<T> {
	int getTotalPages();
	long getTotalElements();
	<S> Page<S> map(Converter<? super T, ? extends S> converter);
}
```

```java
//Chunk类收到pageable参数的影响，提供了一大堆和分页有关的方法。

///PageImpl分页实现继承Check类就相当于拥有了这些方法，从而底层依赖这pageable,content
abstract class Chunk<T> implements Slice<T>, Serializable {

	private static final long serialVersionUID = 867755909294344406L;

	private final List<T> content = new ArrayList<T>();
	private final Pageable pageable;

	public Chunk(List<T> content, Pageable pageable) {
		Assert.notNull(content, "Content must not be null!");
		this.content.addAll(content);
		this.pageable = pageable;
	}
    
	public int getNumber() {
		return pageable == null ? 0 : pageable.getPageNumber();
	}
    
	public int getSize() {
		return pageable == null ? 0 : pageable.getPageSize();
	}
    
	public int getNumberOfElements() {
		return content.size();
	}
    
	public boolean hasPrevious() {
		return getNumber() > 0;
	}
    
	public boolean isFirst() {
		return !hasPrevious();
	}

	public boolean isLast() {
		return !hasNext();
	}
    
	public Pageable nextPageable() {
		return hasNext() ? pageable.next() : null;
	}
    
	public Pageable previousPageable() {
		if (hasPrevious()) {
			return pageable.previousOrFirst();
		}
		return null;
	}
    
	public boolean hasContent() {
		return !content.isEmpty();
	}
    
	public List<T> getContent() {
		return Collections.unmodifiableList(content);
	}
    
	public Sort getSort() {
		return pageable == null ? null : pageable.getSort();
	}
}
```

