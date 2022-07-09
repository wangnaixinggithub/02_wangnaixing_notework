# PagingAndSortingRepository

# 1、PagingAndSortingRepository

```java
@NoRepositoryBean
public interface PagingAndSortingRepository<T, ID extends Serializable> extends CrudRepository<T, ID> {
    
	//对查询出来的全部数据进行排序
	Iterable<T> findAll(Sort sort);
    
	//对查询出来的全部数据进行分页
	Page<T> findAll(Pageable pageable);
    
}
```

# 2、Sort

```java
public class Sort implements Iterable<org.springframework.data.domain.Sort.Order>, Serializable {
	
    public static final Direction DEFAULT_DIRECTION = Direction.ASC;
	
    private final List<Order> orders
        
  	 //静态内部类Order     
 	 public static class Order implements Serializable {}
    
    //Direction枚举
 	 public static enum Direction { ASC, DESC;｝
｝
```

```java
public class Sort implements Iterable<org.springframework.data.domain.Sort.Order>, Serializable {
 
    // 构造方式1：
	public Sort(List<Order> orders) {
      
		if (null == orders || orders.isEmpty()) {
			throw new IllegalArgumentException("You have to provide at least one sort property to sort by!");
		}
      
     
		this.orders = orders;
	}
    //构造方式2：
    public Sort(Order... orders) {
		this(Arrays.asList(orders));
	}
    
    //构造方式3：
    public Sort(Direction direction, List<String> properties) {
		
		if (properties == null || properties.isEmpty()) {
			throw new IllegalArgumentException("You have to provide at least one property to sort by!");
		}
	
		this.orders = new ArrayList<Order>(properties.size());

		for (String property : properties) {
			this.orders.add(new Order(direction, property));
		}
	}
    
    //构造方法4：
    public Sort(Direction direction, String... properties) {
		this(direction, properties == null ? new ArrayList<String>() : Arrays.asList(properties));
	}
    
    //链式添加Sort
    public Sort and(Sort sort) {
		if (sort == null) {
			return this;
		}
        
		ArrayList<Order> these = new ArrayList<Order>(this.orders);
		
		for (Order order : sort) {
			these.add(order);	
		}
    
		return new Sort(these);
	}
    
    
｝
```

```java
public class Sort implements Iterable<org.springframework.data.domain.Sort.Order>, Serializable {	
public static class Order implements Serializable {
        private final Direction direction;
		private final String property;
		private final boolean ignoreCase;
		private final NullHandling nullHandling;
        
public Order(Direction direction, String property) {
		this(direction, property, DEFAULT_IGNORE_CASE, null);
	}
     
  }   
}
```

```java
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable  {
     private static final Object[] EMPTY_ELEMENTDATA = {};

     public ArrayList(Collection<? extends E> c) {
    	//1.把集合转为数组
        elementData = c.toArray();
      
    	//2.判断了数组长度不能为空才处理
        if ((size = elementData.length) != 0) {
 			
            //3.执行数组复制操作
            if (elementData.getClass() != Object[].class)
                
                elementData = Arrays.copyOf(elementData, size, Object[].class);
       		 } else {
           
            	// 如果为0，就给一个空集合{}
          	  	this.elementData = EMPTY_ELEMENTDATA;
       		 }
    	}
}
```

# 3、Pageable

```java
public interface Pageable {
		//获取 当前页
        int getPageNumber();
    	
        // 获取 每页大小
        int getPageSize();
    	
       // 获取偏移量
    	int getOffset();
    	
      // 获取排序对象
    	Sort getSort();
		
    	//下一页
        Pageable next();
    	
    	//上一页 OR 首页
    	Pageable previousOrFirst();
    	
    	// 首页
    	Pageable first();
		
    	//上一页?
   		boolean hasPrevious();
}
```

```java
public abstract class AbstractPageRequest implements Pageable, Serializable {
    private final int page;
	private final int size;
    
    public AbstractPageRequest(int page, int size) {
    
		if (page < 0) {
			throw new IllegalArgumentException("Page index must not be less than zero!");
		}
      
		if (size < 1) {
			throw new IllegalArgumentException("Page size must not be less than one!");
		}
        
		this.page = page;
        
		this.size = size;
	}
 
    public int getPageSize() {return size;}
    
    public int getPageNumber() {return page;}
    
    public int getOffset() {return page * size;}
    
    public boolean hasPrevious() {return page > 0;}
    
    public Pageable previousOrFirst() {return hasPrevious() ? previous() : first();}
    
 
    @Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + page;
		result = prime * result + size;
		return result;
	}
    
    @Override
	public boolean equals(Object obj) {
      
		if (this == obj) {
			return true;
		}
     
		if (obj == null || getClass() != obj.getClass()) {
			return false;
		}
     
		AbstractPageRequest other = (AbstractPageRequest) obj;
		return this.page == other.page && this.size == other.size;
	}
｝
```

```java
public class PageRequest extends AbstractPageRequest {
    private final Sort sort;
    
    public PageRequest(int page, int size, Sort sort) {
		super(page, size);
		this.sort = sort;
	}
    
    public PageRequest(int page, int size) {
        
		this(page, size, null);
		
    }
	public PageRequest(int page, int size, Direction direction, String... properties) {
		this(page, size, new Sort(direction, properties));
        
    public Sort getSort() {return sort;}
        
     //当前页进行减1     
    public PageRequest previous() {return getPageNumber() == 0 ? this : new PageRequest(getPageNumber() - 1, getPageSize(), getSort());
                                   
       //当前页进行加1                            
    public Pageable next() {return new PageRequest(getPageNumber() + 1, getPageSize(), getSort());}	  
                                   
      //把当前页设置为0了                                            
   	public Pageable first() {return new PageRequest(0, getPageSize(), getSort());}                    
        
｝
```



