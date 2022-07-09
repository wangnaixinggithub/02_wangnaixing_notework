# Spring-Assert

## 1、Validation MethodArg,Throw New Exception

```java
public abstract class Assert {
   
    //断言布尔表示式必是true，如果结果为false,则抛出参数不合法异常。
	public static void state(boolean expression, String message) {
		if (!expression) {
			throw new IllegalStateException(message);
		}
	}
 
    //断言表达式expression结果必是true，如果为false,抛出参数不合法异常。
    public static void isTrue(boolean expression, String message){
        if (!expression) {
			throw new IllegalArgumentException(message);
		}
    }
   
    //断言对象必是空null，如果不是null,抛出参数不合法异常。
    public static void isNull(@Nullable Object object, String message) {
		if (object != null) {
			throw new IllegalArgumentException(message);
		}
	}
  
    //断音对象必是不为空，如果为null,抛出参数不合法异常。
    public static void notNull(@Nullable Object object, String message) {
		if (object == null) {
			throw new IllegalArgumentException(message);
		}
	}
   
    //断言文本不是null，也不是并且长度大于0，如果是null或者长度为0，抛出参数不合法异常。
    //综合下面分析，可以知道方法逻辑为：断言字符串必须不为空并且不是一个空串的逻辑。
   	public static void hasLength(@Nullable String text, String message) {
		if (!StringUtils.hasLength(text)) {
			throw new IllegalArgumentException(message);
		}
	} 
   
    //断音字符串是有输入内容的字符串，如果不是则抛出参数不合格异常。
    public static void hasText(@Nullable String text, String message)｛
        if (!StringUtils.hasText(text)) {
			throw new IllegalArgumentException(message);
		}
     ｝
	
         //断言字符串textToSearch必须不包含substring,如果不是则抛出参数不合格异常。
    public static void doesNotContain(@Nullable String textToSearch, String substring, String message) {
		if (StringUtils.hasLength(textToSearch) && StringUtils.hasLength(substring) &&
				textToSearch.contains(substring)) {
			throw new IllegalArgumentException(message);
		}
	} 
    
｝
```

```java
public abstract class StringUtils {	
    //检查给定的String既不是null也不是长度为 0。
	public static boolean hasLength(@Nullable String str) {
		return (str != null && !str.isEmpty());
	}
｝	
```

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    //String的底层是通过字节数组value实现的
    private final byte[] value;    
    public boolean isEmpty() {
        //判断字节数组长度为0吗？
        return value.length == 0;
    }    
}
```

- 当str为空串的时候，调用String类的实例方法isEmpty()方法，返回true.证明当字符串为空串时，底层的字节数组长度为0.

```java
    @SneakyThrows
    @Test
    public void test(){
        String str = "";
        System.out.println(str.isEmpty());//true
    }
```

```java
public abstract class StringUtils {		
    //检查给定的String是否包含实际的text 。
    //更具体地说，如果String不为null 、长度大于 0 且包含至少一个非空白字符，则此方法返回true 。
	public static boolean hasText(@Nullable String str) {
		return (str != null && !str.isEmpty() && containsText(str));
	}
	private static boolean containsText(CharSequence str) {
        //获取字符串长度
		int strLen = str.length();
      
        //这个for循环是为了，验证该字符串字符，存在有空格字符以外的字符时，返回true.
		for (int i = 0; i < strLen; i++) {
			if (!Character.isWhitespace(str.charAt(i))) {
				return true;  
			}
		}
        
		return false;
	}
｝
```

- 接口字符序列定义了根据索引值获取字符序列的值。

```java
public interface CharSequence {
	//字符序列，根据索引获取字符序列的字符
	char charAt(int index);
｝
```

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
     //实现了字符序列接口，重写chatAt()方法，虽然逻辑看不懂。
    public char charAt(int index) {
        if (isLatin1()) {
            return StringLatin1.charAt(value, index);
        } else {
            return StringUTF16.charAt(value, index);
        }
    }
｝
```

```java
public final class Character implements java.io.Serializable, Comparable<Character> {
public static boolean isWhitespace(char ch) {
        return isWhitespace((int)ch);
    }
 ｝   
```

- 只有是空字符的时候，返回true.

```java
    @SneakyThrows
    @Test
    public void test(){
        boolean whitespace = Character.isWhitespace(' '); //true
        System.out.println(whitespace);
    }
```

- 综上分析StringUtils工具类的hasText()方法，在如下三种情况下，

```java
    @SneakyThrows
    @Test
    public void test(){
        String str = "";  //长度为0
        String str1 = null; //没有赋于空间
        String str2 = "    "; //长度为4
        System.out.println(StringUtils.hasText(str)); // false
        System.out.println(StringUtils.hasText(str1)); // false
        System.out.println(StringUtils.hasText(str2)); //false
    }
```

## 2 、Other Method

```java
public abstract class Assert {
     
    //断音数组不为空，并且数组长度不等于0，如果不是抛出参数不合格异常。
	 public static void notEmpty(@Nullable Object[] array, String message) {
		if (ObjectUtils.isEmpty(array)) {
			throw new IllegalArgumentException(message);
		}
	}
  
    //断音数组不是一个空元素的数组，如果数组有元素为null,则抛出参数不合格异常。这里注意没有考虑array 为空的情况。！ 	
    public static void noNullElements(@Nullable Object[] array, String message) {
		if (array != null) {
			for (Object element : array) {
				if (element == null) {
					throw new IllegalArgumentException(message);
				}
			}
		}
	}
   
    //断言集合不是null也不是空集合，如果这个集合是空或者是一个空集合就抛出参数不合格异常。
   public static void notEmpty(@Nullable Collection<?> collection, String message) {
		if (CollectionUtils.isEmpty(collection)) {
			throw new IllegalArgumentException(message);
		}
	}
   //断音对象obj是type类型，如果对象obj不是type类型，就抛出参数不合格异常。
   public static void isInstanceOf(Class<?> type, @Nullable Object obj, String message) {
       //断音type不为空
		notNull(type, "Type to check against must not be null");
       //判断obj是不是type类的实例？
		if (!type.isInstance(obj)) {
            //不是，进入实例检查失败方法。
			instanceCheckFailed(type, obj, message);
		}
	}
 
    private static void instanceCheckFailed(Class<?> type, @Nullable Object obj, @Nullable String msg) {
		String className = (obj != null ? obj.getClass().getName() : "null");
		String result = "";
		boolean defaultMessage = true;
       //拼装异常结果。
		if (StringUtils.hasLength(msg)) {
			if (endsWithSeparator(msg)) {
				result = msg + " ";
			}
			else {
				result = messageWithTypeName(msg, className);
				defaultMessage = false;
			}
		}
		if (defaultMessage) {
			result = result + ("Object of class [" + className + "] must be an instance of " + type);
		}
       //抛出参数不合法异常。
		throw new IllegalArgumentException(result);
	}

    //判断父类superType的子类型subType是不是？  
  public static void isAssignable(Class<?> superType, @Nullable Class<?> subType, String message) {
      //父类型superType不为空
		notNull(superType, "Super type to check against must not be null");
      
		if (subType == null || !superType.isAssignableFrom(subType)) { //父类型和子类型无相关，进入可分配检查错误。
			assignableCheckFailed(superType, subType, message);
		}
	}
    
    
private static void assignableCheckFailed(Class<?> superType, @Nullable Class<?> subType, @Nullable String msg) {
		String result = "";
		boolean defaultMessage = true;
        //拼装异常结果。
		if (StringUtils.hasLength(msg)) {
			if (endsWithSeparator(msg)) {
				result = msg + " ";
			}
			else {
				result = messageWithTypeName(msg, subType);
				defaultMessage = false;
			}
		}
		if (defaultMessage) {
			result = result + (subType + " is not assignable to " + superType);
		}
      //抛出参数不合法异常。
		throw new IllegalArgumentException(result);
	}
｝
```

```java
public abstract class ObjectUtils {
	public static boolean isEmpty(@Nullable Object[] array) {
		return (array == null || array.length == 0);
	}
｝	
```

```java
public abstract class CollectionUtils {
	public static boolean isEmpty(@Nullable Collection<?> collection) {
        //集合为空或者是一个空集合，结果都是true.
		return (collection == null || collection.isEmpty());
	}
}
```

