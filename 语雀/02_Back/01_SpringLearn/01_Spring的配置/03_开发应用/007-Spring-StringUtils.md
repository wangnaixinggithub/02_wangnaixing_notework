# Spring-StringUtils

> Spring对于字符串的操作也提供了自己的工具类，StringUtils

# 1、#quote()

- 可以给你的字符串`自动的加上单引号`,这可比你自定拼接优雅多了,点开源码，其实他也是在做单引号的拼接。多了一步判空而已！

```java
    @Test
    public void test(){
        String parameter = "wangnaixing";
        String quote = StringUtils.quote(parameter);
        System.out.println(quote); //'wangnaixing'
    }
```

```java
public abstract class StringUtils {
	@Nullable
	public static String quote(@Nullable String str) {
		return (str != null ? "'" + str + "'" : null);
	}
}	
```

# 2、#collectionToDelimitedString()

- 可以把集合转为字符串，并指定分隔符.

```java
    @Test
    public void test(){
        
        Map<String, Object> map1 = new HashMap<>();
        map1.put("key1", "value1");
        map1.put("key2", "value2");
        map1.put("key3", "value3");
        map1.put("key4", "value4");
        
        Set<String> keySet = map1.keySet();
      
        String s = StringUtils.collectionToDelimitedString(keySet, ",");
        String s1 = StringUtils.collectionToCommaDelimitedString(keySet);
      
        System.out.println(s);  //key1,key2,key3,key4
        System.out.println(s1);  //key1,key2,key3,key4 默认 分隔符为,

    }
```

```java
public abstract class StringUtils {
	public static String collectionToDelimitedString(@Nullable Collection<?> coll, String delim) {
		return collectionToDelimitedString(coll, delim, "", "");
	}
    
    public static String arrayToDelimitedString(@Nullable Object[] arr, String delim) {
		
        if (ObjectUtils.isEmpty(arr)) {
			return "";
		}
        
		if (arr.length == 1) {
			return ObjectUtils.nullSafeToString(arr[0]);
		}
        
		//方法的核心还是做了遍历的！ sb.append
		StringBuilder sb = new StringBuilder();
		for (int i = 0; i < arr.length; i++) {
			if (i > 0) {
				sb.append(delim);
			}
			sb.append(arr[i]);
		}
        
		return sb.toString();
	}
}	
```

# 3、#commaDelimitedListToStringArray()

- 可以将逗号分隔的字符串转为数组

```java
public abstract class StringUtils {
    public static String[] commaDelimitedListToStringArray(@Nullable String str) {
            return delimitedListToStringArray(str, ",");
        }
  ｝      
```

```java
    @Test
    public void test(){
         String str = "key1,key2,key3";
        String[] strings = StringUtils.commaDelimitedListToStringArray(str);
        System.out.println(Arrays.toString(strings)); //[key1, key2, key3]

    }
}
```

# 4、#trimXXX()

- 修剪字符串的相关API

```properties
trimWhitespace()=前后空格
trimAllWhitespace()=全部空格
trimLeadingWhitespace()=开始空格
trimTrailingWhitespace()=结尾空格

trimLeadingCharacter()=开始某字符
trimTrailingCharacter()=结尾某字符
```

```java
public abstract class StringUtils {
    public static String trimWhitespace(String str) {

            //1.为null则不处理
            if (!hasLength(str)) {
                return str;
            }

            //2.找到没有 '' 的字符串beginIndex endIndex
            int beginIndex = 0;
            int endIndex = str.length() - 1;

            while (beginIndex <= endIndex && Character.isWhitespace(str.charAt(beginIndex))) {
                beginIndex++;
            }

            while (endIndex > beginIndex && Character.isWhitespace(str.charAt(endIndex))) {
                endIndex--;
            }
            //3. substring() 截取子串
            return str.substring(beginIndex, endIndex + 1);
        }	
    public static String trimAllWhitespace(String str) {
           //1.空串不处理
            if (!hasLength(str)) {
                return str;
            }

            //2.把不是''字符的拼在一起
            int len = str.length();
            StringBuilder sb = new StringBuilder(str.length());
            for (int i = 0; i < len; i++) {
                char c = str.charAt(i);
                if (!Character.isWhitespace(c)) {
                    sb.append(c);
                }
            }
            return sb.toString();
        }
    public static String trimLeadingWhitespace(String str) {
            //trimWhitespace() find beginIndex
            if (!hasLength(str)) {
                return str;
            }

            int beginIdx = 0;
            while (beginIdx < str.length() && Character.isWhitespace(str.charAt(beginIdx))) {
                beginIdx++;
            }
            return str.substring(beginIdx);


        }
    public static String trimTrailingWhitespace(String str) {
            //trimWhitespace() find endIndex
            if (!hasLength(str)) {
                return str;
            }

            int endIdx = str.length() - 1;
            while (endIdx >= 0 && Character.isWhitespace(str.charAt(endIdx))) {
                endIdx--;
            }

            return str.substring(0, endIdx + 1);
        }
    public static String trimLeadingCharacter(String str, char leadingCharacter) {
            if (!hasLength(str)) {
                return str;
            }
            //比较的字符变了而已
            int beginIdx = 0;
            while (beginIdx < str.length() && leadingCharacter == str.charAt(beginIdx)) {
                beginIdx++;
            }
            return str.substring(beginIdx);
    }
    public static String trimTrailingCharacter(String str, char trailingCharacter) {
            if (!hasLength(str)) {
                return str;
            }

            //比较的字符变了而已
            int endIdx = str.length() - 1;
            while (endIdx >= 0 && trailingCharacter == str.charAt(endIdx)) {
                endIdx--;
            }
            return str.substring(0, endIdx + 1);
        }
}
```

```java
    @Test
    public void test_trimXXX() {
        String trimFirstEndBlank = " 王乃醒 ";
        String trimAllBlank= " 王 乃 醒 ";
        String trimFirstBlank = " 王乃醒";
        String trimEndBlank = "王乃醒 ";
        String trimFirstChar = "W王乃醒";
        String trimEndChar = "王乃醒W";

        String shouldGetStr = "王乃醒";

        assertEquals(shouldGetStr, StringUtils.trimWhitespace(trimFirstEndBlank));
        assertEquals(shouldGetStr,StringUtils.trimAllWhitespace(trimAllBlank));
        assertEquals(shouldGetStr,StringUtils.trimLeadingWhitespace(trimFirstBlank));
        assertEquals(shouldGetStr,StringUtils.trimTrailingWhitespace(trimEndBlank));
        assertEquals(shouldGetStr,StringUtils.trimLeadingCharacter(trimFirstChar,'W'));
        assertEquals(shouldGetStr,StringUtils.trimTrailingCharacter(trimEndChar,'W'));

    }
```

# 5、#containsWhitespace()

- 判断字符串是否含有空格

```java
public abstract class StringUtils {	
	public static boolean containsWhitespace(@Nullable String str) {
		return containsWhitespace((CharSequence) str);
	}
	
	public static boolean containsWhitespace(@Nullable CharSequence str) {
		if (!hasLength(str)) {
			return false;
		}
		//遍历字节序列，+ if判断 isWhitespace()
		int strLen = str.length();
		for (int i = 0; i < strLen; i++) {
			if (Character.isWhitespace(str.charAt(i))) {
				return true;
			}
		}
		return false;
	}
}
```

```java
    @Test
    public void test_containsWhitespace() {
        
        String trimFirstEndBlank = " 王乃醒 ";
        String trimAllBlank= " 王 乃 醒 ";
        String trimFirstBlank = " 王乃醒";
        String trimEndBlank = "王乃醒 ";
     
        assertTrue(StringUtils.containsWhitespace(trimFirstEndBlank));
        assertTrue(StringUtils.containsWhitespace(trimAllBlank));
        assertTrue(StringUtils.containsWhitespace(trimFirstBlank));
        assertTrue(StringUtils.containsWhitespace(trimEndBlank));
    }
```

# 6、#hasLength()

- 字符串不为空 则返回true，不为空包含两层含义，其一引用不为空，字符串长度大于0(有字符记录)

```java
public abstract class StringUtils {
   //重载
	public static boolean hasLength(@Nullable CharSequence str) {
		return (str != null && str.length() > 0);
	}
	public static boolean hasLength(@Nullable String str) {
		return (str != null && !str.isEmpty());
	}
}
```

```java
 public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
     public boolean isEmpty() {
            return value.length == 0;
        }
}
```

```java
    @Test
    public void test_hasLength() {
        // You Need Know: String  implements   CharSequence
        String nullStr = null;
        String nullStr2 = "";

        assertFalse(StringUtils.hasLength(nullStr));
        assertFalse(StringUtils.hasLength(nullStr2));
    }
```

# 7、#hasText()

- 必须是有内容的字符串，才能得到`True`

```java
public abstract class StringUtils {	
    
	public static boolean hasText(@Nullable CharSequence str) {
		return (str != null && str.length() > 0 && containsText(str));
	}

	public static boolean hasText(@Nullable String str) {
		return (str != null && !str.isEmpty() && containsText(str));
	}
	//和 containsWhitespace() 逻辑一样
    private static boolean containsText(CharSequence str) {
		int strLen = str.length();
		for (int i = 0; i < strLen; i++) {
			if (!Character.isWhitespace(str.charAt(i))) {
				return true;
			}
		}
		return false;
	}
}
```

```java
    @Test
    public void test_hasText() {
        String nullStr = null;
        String nullStr2 = ""; 
        String containBlank = " 王乃醒 ";

        assertFalse(StringUtils.hasLength(nullStr));
        assertFalse(StringUtils.hasLength(nullStr2));
        assertFalse(StringUtils.hasText(containBlank));
    }

```

# 8、#startsWithIgnoreCase() 

- 字符串包含，你指定的开头。

```java
    @Test
    public void test_startsWithIgnoreCase() {
        String str = "王乃醒";
        assertTrue(StringUtils.startsWithIgnoreCase(str,"王"));
    }
```

```java
public abstract class StringUtils {	
	public static boolean startsWithIgnoreCase(@Nullable String str, @Nullable String prefix) {
		return (str != null && prefix != null && str.length() >= prefix.length() &&
				str.regionMatches(true, 0, prefix, 0, prefix.length()));
	}
    
    
}
```

```java
 public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
 	
    // 区域匹配
   public boolean regionMatches(boolean ignoreCase, int toffset,String other, int ooffset, int len) {
        char ta[] = value;
        int to = toffset;
        char pa[] = other.value;
        int po = ooffset;
            
        if ((ooffset < 0) || (toffset < 0)
                || (toffset > (long)value.length - len)
                || (ooffset > (long)other.value.length - len)) {
            return false;
        }
         
        while (len-- > 0) {
            char c1 = ta[to++];
            char c2 = pa[po++];
            if (c1 == c2) {
                continue;
            }
            
            if (ignoreCase) {
                char u1 = Character.toUpperCase(c1);
                char u2 = Character.toUpperCase(c2);
                if (u1 == u2) {
                    continue;
                }
                if (Character.toLowerCase(u1) == Character.toLowerCase(u2)) {
                    continue;
                }
            }
            
            return false;
        }
         
        return true;
    }
}
```



# 9、#endsWithIgnoreCase()

- 字符串包含，你给的结尾。

```java
    @Test
    public void test_endsWithIgnoreCase() {
        String str = "王乃醒";
        assertTrue(StringUtils.endsWithIgnoreCase(str,"醒"));
    }
```

```java
public abstract class StringUtils {	
	public static boolean endsWithIgnoreCase(@Nullable String str, @Nullable String suffix) {
		return (str != null && suffix != null && str.length() >= suffix.length() &&
				str.regionMatches(true, str.length() - suffix.length(), suffix, 0, suffix.length()));
	}
}
```

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence { 
	public boolean regionMatches(boolean ignoreCase, int toffset,String other, int ooffset, int len) {...}
}
```

# 10、#substringMatch()

- 子字符串匹配。

```java
     @Test
    public void test_substringMatch() {
       String str = "王乃醒现在是实习生";
       //     王 乃 醒 现 在  是  实 习 生
        //    0  1 2  3  4  5  6  7  8
       assertTrue(StringUtils.substringMatch(str, 6, "实习生"));
    }
```

```java
public abstract class StringUtils {	
	
    public static boolean substringMatch(CharSequence str, int index, CharSequence substring) {
		 //1.必须是有效的比较位置
        if (index + substring.length() > str.length()) {
			return false;
		}
        
        //2.做匹配
		for (int i = 0; i < substring.length(); i++) {
			if (str.charAt(index + i) != substring.charAt(i)) {
				return false;
			}
		}
        
		return true;
	}
}
```

# 11、#countOccurrencesOf()

- 统计某子串出现次数

```java
    @Test
    public void test_countOccurrencesOf() {
          String str = "王乃醒 王乃醒 王乃醒 王乃醒 王乃醒 王乃醒";
          assertEquals(6,StringUtils.countOccurrencesOf(str,"王乃醒"));
    }
```

```java
public abstract class StringUtils {
    
	public static int countOccurrencesOf(String str, String sub) {
        //1.str sub Not Empty Str
		if (!hasLength(str) || !hasLength(sub)) {
			return 0;
		}
		
        //2. Find Count
		int count = 0;
		int pos = 0;
		int idx;
		while ((idx = str.indexOf(sub, pos)) != -1) {
			++count;
			pos = idx + sub.length();
		}
		return count;
	}
}
```

# 12、#replace()

- 字符串替换操作

```java
public abstract class StringUtils {
	public static String replace(String inString, String oldPattern, @Nullable String newPattern) {
		
        if (!hasLength(inString) || !hasLength(oldPattern) || newPattern == null) {
			return inString;
		}
        
		int index = inString.indexOf(oldPattern);
		if (index == -1) {
			// no occurrence -> can return input as-is
			return inString;
		}

		int capacity = inString.length();
		if (newPattern.length() > oldPattern.length()) {
			capacity += 16;
		}
        
		StringBuilder sb = new StringBuilder(capacity);

		int pos = 0;  // our position in the old string
		int patLen = oldPattern.length();
		
        while (index >= 0) {
			sb.append(inString, pos, index);
			sb.append(newPattern);
			pos = index + patLen;
			index = inString.indexOf(oldPattern, pos);
		}

		// append any characters to the right of a match
		sb.append(inString, pos, inString.length());
        
		return sb.toString();
	}
}
```

```java
    @Test
    public void test_replace() {
        String str = "王乃醒 王乃醒 王乃醒 王乃醒 王乃醒 王乃醒";
        assertEquals("黄洁莹 黄洁莹 黄洁莹 黄洁莹 黄洁莹 黄洁莹",
                StringUtils.replace(str,"王乃醒","黄洁莹"));
    }
```

# 13、#delete()

- 删除给定子串

```java
    @Test
    public void test_delete() {
        String str = "DELETE王乃醒DELETE";
        assertEquals("王乃醒",StringUtils.delete(str, "DELETE"));
    }
```

```java
public abstract class StringUtils {
	public static String delete(String inString, String pattern) {
		return replace(inString, pattern, "");
	}
}
```

# 14 、deleteAny()

- 删除给定子串的全部

```java
public abstract class StringUtils {
    
	public static String deleteAny(String inString, @Nullable String charsToDelete) {
        //1.保证要删除串，删除串不为空
		if (!hasLength(inString) || !hasLength(charsToDelete)) {
			return inString;
		}
		//2.删除charsToDelete 中出现有的字符串
		int lastCharIndex = 0;
		char[] result = new char[inString.length()];
		for (int i = 0; i < inString.length(); i++) {
			char c = inString.charAt(i);
			if (charsToDelete.indexOf(c) == -1) {
				result[lastCharIndex++] = c;
			}
		}
        
		if (lastCharIndex == inString.length()) {
			return inString;
		}
		return new String(
            result, 0, lastCharIndex);
	}
}
```

# 15、#quoteIfString()

- 不是字符就原样输出，如果是，就加上单引号。

```java
    @Test
    public void test_quoteIfString() {
        String dateStr = "2022-12-01";
        Integer num1 = 1;
        assertEquals("'2022-12-01'",StringUtils.quoteIfString(dateStr));
        assertEquals(num1,StringUtils.quoteIfString(1));
    }

```

```java
public abstract class StringUtils {
    @Nullable
	public static Object quoteIfString(@Nullable Object obj) {
		return (obj instanceof String ? quote((String) obj) : obj);
	}
}
```

# 16、#unqualify()

- 从全类名中解析出来类名

```java
  @Test
    public void test_unqualify() {
         String qualifiedName = "com.wnx.mall.User";
         String qualifiedName2 = "java.util.Properties";
         assertEquals("User",StringUtils.unqualify(qualifiedName));
         assertEquals("Properties",StringUtils.unqualify(qualifiedName2));
    }

```

```java
public abstract class StringUtils {

    public static String unqualify(String qualifiedName) {
		return unqualify(qualifiedName, '.');
	}
    
	public static String unqualify(String qualifiedName, char separator) {
        //截取最后一个.出现的内容
		return qualifiedName.substring(qualifiedName.lastIndexOf(separator) + 1);
	}
}
```

# 17、#uncapitalize() #capitalize()

- 首字母小写 And 首字母大写

```java
    @Test
    public void test_uncapitalize() {
          String str1 = "Wangnaixing";
          assertEquals("wangnaixing",StringUtils.uncapitalize(str1));
    }
  
```

```java
   @Test
    public void test_capitalize() {
        String str1 = "wNX";
        assertEquals("WNX",StringUtils.capitalize(str1));
    }
```

```java
public abstract class StringUtils {
	public static String uncapitalize(String str) {
		return changeFirstCharacterCase(str, false);
	}
    
   public static String capitalize(String str) {
		return changeFirstCharacterCase(str, true);
	}
    
   private static String changeFirstCharacterCase(String str, boolean capitalize) {
		if (!hasLength(str)) {
			return str;
		}

		char baseChar = str.charAt(0);
		char updatedChar;
		
       //capitalize 决定 大写还是小写
       if (capitalize) {
			updatedChar = Character.toUpperCase(baseChar);
		}
		else {
			updatedChar = Character.toLowerCase(baseChar);
		}
       
       
		if (baseChar == updatedChar) {
			return str;
		}

		char[] chars = str.toCharArray();
		chars[0] = updatedChar;
		return new String(chars);
	
   }
}
```

# 18、#getFilename()

- 获取文件名

```java
    @Test
    public void test_getFilename() {
        String dirPath = "D:/BaiduNetdiskDownload/Mybatis";
        String filePath = "D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.1.zip";
        assertEquals("Mybatis",StringUtils.getFilename(dirPath));
        assertEquals("mybatis-3.4.1.zip",StringUtils.getFilename(filePath));
    }
```

```java
public abstract class StringUtils {	
    private static final char FOLDER_SEPARATOR_CHAR = '/';
    
	@Nullable
	public static String getFilename(@Nullable String path) {
		if (path == null) {
			return null;
		}
		//获取最后一个/索引
		int separatorIndex = path.lastIndexOf(FOLDER_SEPARATOR_CHAR);
 		//subString()
		return (separatorIndex != -1 ? path.substring(separatorIndex + 1) : path);
	}
}
```

# 19、#getFilenameExtension()

- 获取文件后缀名

```java
  @Test
    public void test() {
        String filePath1 = "D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.1.zip";
        String filePath2 = "D:/BaiduNetdiskDownload/Mybatis/wangnaixing.txt";

        assertEquals("zip",StringUtils.getFilenameExtension(filePath1));
        assertEquals("txt",StringUtils.getFilenameExtension(filePath2));

    }
```

# 20、#stripFilenameExtension()

- 获取文件名后缀前面部分

```java
    @Test
    public void test_stripFilenameExtension() {
        String filePath1 = "D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.1.zip";
        String filePath2 = "D:/BaiduNetdiskDownload/Mybatis/wangnaixing.txt";

        assertEquals("D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.1",StringUtils.stripFilenameExtension(filePath1));
        assertEquals("D:/BaiduNetdiskDownload/Mybatis/wangnaixing",StringUtils.stripFilenameExtension(filePath2));

    }
```

# 21、#applyRelativePath()

- 组装相对路径

```java
    @Test
    public void test_applyRelativePath() {
        String applyRelativePath = StringUtils.applyRelativePath("D:/BaiduNetdiskDownload/Mybatis/", "mybatis-3.4.1.zip");
        assertEquals("D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.1.zip",applyRelativePath);
    }
```

# 22、#pathEquals()

- 比较两个路径是否相等

```java
    @Test
    public void test() {
       String path1 = "D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.1.zip";
       String path2 = "D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.1.zip";
       String path3 = "D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.2.zip";
       assertTrue(StringUtils.pathEquals(path1,path2));
       assertFalse(StringUtils.pathEquals(path1, path3));
    }
```

