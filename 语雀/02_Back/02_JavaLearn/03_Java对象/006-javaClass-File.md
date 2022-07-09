# javaClass-File

# 1、MemberVariables

```java
public class Fileimplements Serializable, Comparable<File>{
    //操作系统平台本地的文件系统
    private static final FileSystem fs = DefaultFileSystem.getFileSystem(); 
 
     //路径名字符串
    private final String path;
    
    //路径状态的枚举：无效的，检查的
    private static enum PathStatus { INVALID, CHECKED };
    
    //路径状态
    private transient PathStatus status = null;
    
    //路径前缀的长度（非静态，在实例调用构造器期间可以再给他赋值。）
    private final transient int prefixLength; 
   
    //分隔器（静态成员变量，在类创建的时候就已经赋值了）
    public static final char separatorChar = fs.getSeparator();
    public static final String separator = "" + separatorChar;
    
    //路径分隔器 （静态成员变量，在类创建的时候就已经赋值了）
    public static final char pathSeparatorChar = fs.getPathSeparator();
    public static final String pathSeparator = "" + pathSeparatorChar;
    
} 
```

## 1.1、File.separator

- 对于这些常量，我们可以在构建路径的时候，避免一些硬编码,通过`File.separator`来代替`\\`,如果在Linux系统，`File.separator`又会变成`/`

```java
    @Test
    public void test_FileClass_separator(){

        Assertions.assertEquals("\\",File.separator);

        //通常用于path 分层划分符
        File file1 = new File("D:\\wangnaixing.p12");
        File file2 = new File("D:"+ File.separator+"wangnaixing.p12");

        Assertions.assertTrue(file1.equals(file2));

    }
}
```

- 又如路径分割器，在Linux系统解析为`：`在windows系统解析为`;`

```java
    @Test
    public void test_FileClass_pathSeparator() {
        //并不理解用途
        Assertions.assertEquals(";",File.pathSeparator);
    }

```

- 除去静态的成员常量，因为这些静态常量在类创建的时候就已经存在与这个类中了，有且仅有一份和类绑定。
- 提供的构造方法，本质对`path`和`prefixLength`进行赋值操作。

```java
public class File implements Serializable, Comparable<File>{ 
    //路径名字符串
 	private final String path; 
    
    //路径前缀的长度（非静态，在实例调用构造器期间可以再给他赋值。）
    private final transient int prefixLength; 
}
```

# 2、ConstructorMethod

## 2.1、PrivateConstructorMethod

```java
public class File implements Serializable, Comparable<File>{	
   
    //提供私有的构造方法，对路径名称和文件前缀长度进行直接赋值。
    private File(String pathname, int prefixLength) {
        this.path = pathname;
        this.prefixLength = prefixLength;
    }	
    
     //提供私有的构造方法，对父文件对象（本质目录），child（本质文件名） 通过文件系统对象解析，还是对路径名称pathname,路径前缀长度复制。
     private File(String child, File parent) {
        //1.断言 父文件路径不为空并且不为空串
        assert parent.path != null;
        assert (!parent.path.equals(""));
		
         //2.根据父路径解析出子目录路径
        this.path = fs.resolve(parent.path, child);
        
         //3.前缀长度就等于父文件的前缀长度
        this.prefixLength = parent.prefixLength;
    }
    
｝
```

```java
package java.io;
abstract class FileSystem {
     //根据父项解析子路径名字符串。两个字符串都必须是正常形式，结果也将是正常形式
	  public abstract String resolve(String parent, String child);
｝
```

## 2.2、PublicConstructorMethod

```java
public class File implements Serializable, Comparable<File>{
    //Use pathname Create File
     public File(String pathname) {}
   
    //Use parentPathName,childPathName Create File
     public File(String parent, String child) {}
    
	//Use parentFile,childPathName Create File
     public File(File parent, String child) {｝
         
    //Use URI Create URI 
    public File(URI uri) {}   
｝
```

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/143-File%E7%B1%BBAPI%E7%9A%84%E5%AD%A6%E4%B9%A0.assets/image-20220521095210697.png)

```java
    @SneakyThrows
    @Test
    public void test_createFile(){
        //1.Use public Constructor

        File file1 = new File(StringUtils.join("D:",File.separator,"wangnaixing.txt"));

        File file2 = new File("D:","wangnaixing.txt");

        File file3 = new File(new File("D:"),"wangnaixing.txt");

        File file4 = new File(new URI("file:/D:/wangnaixing.txt"));


        Assertions.assertTrue(file1.exists());
        Assertions.assertTrue(file2.exists());
        Assertions.assertTrue(file3.exists());
        Assertions.assertTrue(file4.exists());

        
    }
```

> 代码底层：

### 2.2.1、File(String pathname)

```java
public class File implements Serializable, Comparable<File> {	
    
    private static final FileSystem fs = DefaultFileSystem.getFileSystem();
 
      public File(String pathname) {
        //1.路径名为空，抛出空指针异常。
        if (pathname == null) { throw new NullPointerException(); }
       
         //2.fs 将给定的路径名字符串转换为正常形式。
        this.path = fs.normalize(pathname);
      	
         //3.fs计算此路径名字符串前缀的长度
        this.prefixLength = fs.prefixLength(this.path);
    }
｝
```

```java
class DefaultFileSystem {
   
    //返回Window品牌的文件操作系统
	public static FileSystem getFileSystem() { return new WinNTFileSystem();}
｝
```

```java
abstract class FileSystem {
    
    //将给定的路径名字符串转换为正常形式。如果字符串已经是正常形式，那么它会被简单地返回
	  public abstract String normalize(String path);
  
    //计算此路径名字符串前缀的长度。路径名字符串必须是正常格式。
    public abstract int prefixLength(String path);
｝
```

```java
class WinNTFileSystem extends FileSystem { 
 	@Override
    public String normalize(String path) {
        //Window文件系统，具体进行格式化字符串前缀的具体逻辑。
        int n = path.length();
        char slash = this.slash;
        char altSlash = this.altSlash;
        char prev = 0;
        for (int i = 0; i < n; i++) {
            char c = path.charAt(i);
            if (c == altSlash)
                return normalize(path, n, (prev == slash) ? i - 1 : i);
            if ((c == slash) && (prev == slash) && (i > 1))
                return normalize(path, n, i - 1);
            if ((c == ':') && (i > 1))
                return normalize(path, n, 0);
            prev = c;
        }
        if (prev == slash) return normalize(path, n, n - 1);
        return path;
    }
｝
```

### 2.2.2、File(String parent, String child)

```java
public class File implements Serializable, Comparable<File> {
public File(String parent, String child) {
        //子路径不能为空
        if (child == null) { throw new NullPointerException();}
      
    if (parent != null) {
            //都是在调用文件系统的 根据父路径解析子路径返回子路径。 并赋值给成员变量路径参数path
            if (parent.equals("")) {
                this.path = fs.resolve(fs.getDefaultParent(),
                                       fs.normalize(child));
            } else {
                this.path = fs.resolve(fs.normalize(parent),
                                       fs.normalize(child));
            }
        } else {
            this.path = fs.normalize(child);
        }
      
      //调用文件系统，根据抽象路径解析得到获取前缀长度
        this.prefixLength = fs.prefixLength(this.path);
    }
}    
```

### 2.2.3、File(File parent, String child)

```java
public class File implements Serializable, Comparable<File> {	
public File(File parent, String child) {
        //必须保证child不为空
        if (child == null) {throw new NullPointerException();}
        
    if (parent != null) {
            if (parent.path.equals("")) {
                //空串，获取一个默认的父路径，再根据父路径，解析出子路径。
                this.path = fs.resolve(fs.getDefaultParent(),
                                       fs.normalize(child));
            } else {
                //直接根据父路径，解析出子路径。
                this.path = fs.resolve(parent.path,
                                       fs.normalize(child));
            }
        } else {
            //为空就直接根据child，使用文件系统的标注化方法得到文件路径path
            this.path = fs.normalize(child);
        }
       
       //无论如何再根据文件路径path通过文件系统解析出前缀长度。
        this.prefixLength = fs.prefixLength(this.path);
    }
}      
```

### 2.2.4、File(URI uri)

```java
public class File implements Serializable, Comparable<File> {	
public File(URI uri) {
  // Check our many preconditions 检查很多先决条件
        
    if (!uri.isAbsolute())
            throw new IllegalArgumentException("URI is not absolute");
        
    if (uri.isOpaque())
            throw new IllegalArgumentException("URI is not hierarchical");
        
    String scheme = uri.getScheme();
    if ((scheme == null) || !scheme.equalsIgnoreCase("file"))
            throw new IllegalArgumentException("URI scheme is not \"file\"");
        
    if (uri.getRawAuthority() != null)
            throw new IllegalArgumentException("URI has an authority component");
       
    if (uri.getRawFragment() != null)
            throw new IllegalArgumentException("URI has a fragment component");
       
    if (uri.getRawQuery() != null)
            throw new IllegalArgumentException("URI has a query component");
        
    String p = uri.getPath();
       
    if (p.equals(""))
            throw new IllegalArgumentException("URI path component is empty");

        // Okay, now initialize OK 现在开始
        p = fs.fromURIPath(p);
        if (File.separatorChar != '/')
            p = p.replace('/', File.separatorChar);
      
    //还用调用文件系统的标准haul方法得到文件路径子符串path
        this.path = fs.normalize(p);
      
    //再使用文件系统，获取文件前缀长度。
        this.prefixLength = fs.prefixLength(this.path);
    }
}    
```

# 3、InstanceMethod

## 3.1、getName()

```java
public class File implements Serializable, Comparable<File> {   
public String getName() {
        //1.得到分割符所在索引
        int index = path.lastIndexOf(separatorChar);
        
        if (index < prefixLength) return path.substring(prefixLength);
    	
        //2.截取文件名
        return path.substring(index + 1);
    }
}    
```

```java
    @Test
    public void test_FileClass_getName() {
        //1.文件
        File file1 = new File("D:\\BaiduNetdiskDownload\\Mybatis\\mybatis-3-mybatis-3.4.1.zip");
        Assertions.assertEquals("mybatis-3-mybatis-3.4.1.zip",file1.getName());
        
        //2.特殊的文件（目录）
        File file2 = new File("D:\\BaiduNetdiskDownload\\Mybatis");
        Assertions.assertEquals("Mybatis",file2.getName());
    }
```

## 3.2、getParent()

- getParent()方法用来返回父目录的路径字符串表示的！

```java
public class File implements Serializable, Comparable<File>{
public String getParent() {
    	
      //实现逻辑也很清晰，得到最后一次出现/的位置
        int index = path.lastIndexOf(separatorChar);
        if (index < prefixLength) {
            if ((prefixLength > 0) && (path.length() > prefixLength))
                return path.substring(0, prefixLength);
            return null;
        }
    	
      //返回/前面部分
        return path.substring(0, index);
    }
}    
```

```java
    @Test
    public void test_FileClass_getParent() {
        //1.文件
        File file1 = new File("D:\\BaiduNetdiskDownload\\Mybatis\\mybatis-3-mybatis-3.4.1.zip");
        Assertions.assertEquals("D:\\BaiduNetdiskDownload\\Mybatis",file1.getParent());

        //2.特殊的文件（目录）
        File file2 = new File("D:\\BaiduNetdiskDownload\\Mybatis");
        Assertions.assertEquals("D:\\BaiduNetdiskDownload",file2.getParent());

    }
```

## 3.3、getParentFile()

- 返回此抽象路径名的父级抽象路径名。

```java
public class File implements Serializable, Comparable<File>{
  public File getParentFile() {
        //获取此抽象路径名的父抽象路径名的字符串
        String p = this.getParent();
      
      	//判断p不为空
        if (p == null) return null;
       //如果这个类不是File类，通过文件系统，进行格式化路径
        if (getClass() != File.class) {
            p = fs.normalize(p);
        }
       
       //创建一个新的Fiel返回。（私有构造器）
        return new File(p, this.prefixLength);
    }
｝    
```

```java
    @Test
    public void test_FileClass_getParentFile() {
        //1.文件
        File file1 = new File("D:\\BaiduNetdiskDownload\\Mybatis\\mybatis-3-mybatis-3.4.1.zip");
        File expectFile1 = new File("D:\\BaiduNetdiskDownload\\Mybatis");
        Assertions.assertTrue(expectFile1.equals(file1.getParentFile()));

        //2.特殊的文件（目录）
        File file2 = new File("D:\\BaiduNetdiskDownload\\Mybatis");
        File expectFile2 = new File("D:\\BaiduNetdiskDownload");
        Assertions.assertTrue(expectFile2.equals(file2.getParentFile()));
    }
```

## 3.3、getPath()

- 提供将抽象路径名转换为路径名称字符串

```java
 public class File implements Serializable, Comparable<File>{
     private final String path;
     public String getPath() {
         //返回抽象路径名的成员常量 path.
            return path;
        }
 }   
```

```java
    @Test
    public void test_FileClass_getPath() {
        File file = new File("D:\\BaiduNetdiskDownload\\Mybatis\\mybatis-3.4.1.zip");
        Assertions.assertEquals("D:\\BaiduNetdiskDownload\\Mybatis\\mybatis-3.4.1.zip",file.getPath());
    }
```

## 3.4、isAbsolute()

- 判断当前的抽象路径名是不是绝对的路径！

```java
public class File implements Serializable, Comparable<File>{
     public boolean isAbsolute() {
         //使用文件系统fs，判断是绝对的吗？
            return fs.isAbsolute(this);
        }
}
```

```java
    @SneakyThrows
    @Test
    public void test_FileClass_getAbsoluteFile(){
        //1.绝对
        File file1 = new File("D:\\BaiduNetdiskDownload\\Mybatis\\mybatis-3.4.1.zip");
        assertEquals(true,file1.isAbsolute());

        //2.相对
        File file2 = new File("src\\main\\Mybatis\\mybatis-3.4.1.zip");
        assertEquals(false,file2.isAbsolute());

    }
```

## 3.5、getAbsoluteFile() And getAbsoluteFilePath

- 获取绝对路径表示的文件。

```java
public class File implements Serializable, Comparable<File>{ 
    
    public String getAbsolutePath() {
        //文件系统 解析得到path
        return fs.resolve(this);
    }
    
    public File getAbsoluteFile() {
        //先得到绝对路径
        String absPath = getAbsolutePath();
        if (getClass() != File.class) {
             //文件系统格式化路径
            absPath = fs.normalize(absPath);
        }
    
    	//创建一个新的文件对象返回。
        return new File(absPath, fs.prefixLength(absPath));
    }
}    
```

```java
    @Test
    public void test_FileClass_getAbsolutePathAnd_getAbsoluteFile() throws IOException {
        File file1 = new File("D:\\BaiduNetdiskDownload\\Mybatis\\mybatis-3.4.1.zip");
        File file2 = new File("src\\main\\Mybatis\\mybatis-3.4.1.zip");
   
        assertEquals("D:\\BaiduNetdiskDownload\\Mybatis\\mybatis-3.4.1.zip",file1.getAbsolutePath());
        assertEquals("D:\\my_workspace\\java-learn\\src\\main\\Mybatis\\mybatis-3.4.1.zip",file2.getAbsolutePath());
        

    }
```

## 3.6、getCanonicalPath() And getCanonicalFile()

- 有的时候，文件路径可能是不对的，通过该API，解析得到正确的文件路径。

```java
 public class File implements Serializable, Comparable<File>{   
   
     //获取规范化路径名字符串
     public String getCanonicalPath() throws IOException {
            if (isInvalid()) {
                throw new IOException("Invalid file path");
            }
            return fs.canonicalize(fs.resolve(this));
        }
     
     //得到规范的文件
     public File getCanonicalFile() throws IOException {
            String canonPath = getCanonicalPath();
            if (getClass() != File.class) {
                canonPath = fs.normalize(canonPath);
            }
            return new File(canonPath, fs.prefixLength(canonPath));
        }
 }    
```

## 3.7、toURI()

- 我们同样可以把创建出来的文件中获取URI。

```java
public class File implements Serializable, Comparable<File>{
    public URI toURI() {
            try {
                //1.get path
                File f = getAbsoluteFile();
               //2.get scheme
                String sp = slashify(f.getPath(), f.isDirectory());
               
           
                if (sp.startsWith("//"))
                    sp = "//" + sp;
               //3. new 
                return new URI("file", null, sp, null);
                
            } catch (URISyntaxException x) {
             
                throw new Error(x);         // Can't happen
            }
        }
} 
```

```java
public final class URI implements Comparable<URI>, Serializable{
    public URI(String str) throws URISyntaxException {
        new Parser(str).parse(false);
    }
    
    private class Parser {...}   

}
```

```java
    @Test
    public void test_fileClass_toURI() throws IOException, URISyntaxException {
        URI  uri = new URI("file:///D:/BaiduNetdiskDownload/Mybatis/mybatis-3.4.1.zip");
        File file = new File(uri);
        assertEquals(uri,file.toURI());
    }
```

## 3.8、canXXX() setXXX()

- 文件属性由可读可写可执行，我们可获取文件`可读性`，`可写性`，`可执行性`，修改文件的可读可写可执行性。

```java
public class File implements Serializable, Comparable<File>{
	public boolean canRead() {}
    public boolean canWrite(){}
    public boolean canExecute() {}
    
    public boolean setReadOnly(){}                          
  
    public boolean setWritable(boolean writable, boolean ownerOnly){}
    public boolean setWritable(boolean writable) {}
    
    public boolean setReadable(boolean readable, boolean ownerOnly) {}
    public boolean setReadable(boolean readable){}
   
    public boolean setExecutable(boolean executable, boolean ownerOnly){}
    public boolean setExecutable(boolean executable){}
｝
```

## 3.9、isXXX()

- 文件是否存在，文件长度，当前的文件是目录吗？当前的文件是文件吗？当前的文件是隐藏文件吗？当前文件最近修改时间。文件的长度。这些属性都可以获取到。

```java
 @SneakyThrows
    @Test
    public void test(){
        File file3 = new File(new URI("file:///D:/wangnaixing.p12"));
      
        System.out.println(file3.exists());
        System.out.println(file3.isFile());
        System.out.println(file3.isDirectory());
        System.out.println(file3.isHidden());
        System.out.println(file3.lastModified());
        System.out.println(file3.length());
        
        //true
        //true
        //false
        //false
        //1648352873802
        //2241

    }
```

## 3.10、createNewFile()

- 当标识的文件不存在时，可以调用`createNewFile()`方法，创建新的文件。如果创建成功就返回true.
- 如果文件本身已经存在着，执行createNewFile()方法，则会返回false.

```java
 File file3 = new File(new URI("file:///D:/wangnaixing.txt"));
        if (!file3.exists()){
            boolean newFile = file3.createNewFile();
            System.out.println(newFile); //true
        }
```

```java
public class File  implements Serializable, Comparable<File>{
    public boolean createNewFile() throws IOException {
        //1.通过安全管理器检测文件可读性
        SecurityManager security = System.getSecurityManager();
        if (security != null) security.checkWrite(path);
        
        if (isInvalid()) {
            throw new IOException("Invalid file path");
        }
        
        //2.使用文件系统，创建文件。
        return fs.createFileExclusively(path);
    }
}   
```

```java
abstract class FileSystem {
public abstract boolean createFileExclusively(String pathname)
        throws IOException;
}
```

## 3.11、delete() deleteOnExit()

- 通过`#delete()`或者`#deleteOnExit()`方法，可以把存在文件系统中的文件进行删除。
- delete()方法只能用于删除空目录以及文件，对于非空目录，没有一键删除策略的。

```java
    @SneakyThrows
    @Test
    public void test(){
        File file3 = new File(new URI("file:///D:/wangnaixing.txt"));
        boolean delete = file3.delete();
        System.out.println(delete);
    }
```

```java
    @SneakyThrows
    @Test
    public void test(){
        File file3 = new File(new URI("file:///D:/wangnaixing.txt"));
        file3.deleteOnExit();
    }
```

```java
public class File implements Serializable, Comparable<File>{
    public boolean delete() {
        SecurityManager security = System.getSecurityManager();
        	//使用安全管理管理，检查删除路径。
            if (security != null) {
                security.checkDelete(path);
            }
            if (isInvalid()) {
                return false;
            }
            //使用文件管理器删除
            return fs.delete(this);
   }
    
   
 public void deleteOnExit() {
        //安全管理器检查路径
        SecurityManager security = System.getSecurityManager();
        if (security != null) {
            security.checkDelete(path);
        }
        if (isInvalid()) {
            return;
        }
     	//调用DeleteOnExitHooK
        DeleteOnExitHook.add(path);
    }
}    
```

```java
class DeleteOnExitHook {
private static LinkedHashSet<String> files = new LinkedHashSet<>();

    static {
        SharedSecrets.getJavaLangAccess()
            .registerShutdownHook(2 ,
                true ,
                new Runnable() {
                    public void run() {
                       runHooks();
                    }
                }
        );
    }
    
static void runHooks() {
        LinkedHashSet<String> theFiles;
        synchronized (DeleteOnExitHook.class) {
            theFiles = files;
            files = null;
        }
        ArrayList<String> toBeDeleted = new ArrayList<>(theFiles);
        Collections.reverse(toBeDeleted);
        for (String filename : toBeDeleted) {
            // 创建文件对象delete()
            (new File(filename)).delete();
        }
    }   
 ｝
```



## 3.12、list() listFiles()

- 我们可以通过list()方法，得到该目录下面所有的文件和目录文件路径字符串。比如我在D盘的a目录下，拥有3个文件夹分别为a,b,c。三个不同类型的文件a.txt,b.xlsx,c.docx

<img src="143-File%E7%B1%BBAPI%E7%9A%84%E5%AD%A6%E4%B9%A0.assets/image-20220409220535457.png" alt="image-20220409220535457" style="zoom:50%;" />

```java
    @SneakyThrows
    @Test
    public void test(){
      File file = new File("D:\\a");
        String[] list = file.list();
       
        //[a, a.txt, b, b.xlsx, c, c.docx]
        System.out.println(Arrays.toString(list));
        
    }
```

- 调用listFile()方法，通过得到该a目录，所有文件和目录的抽象文件表示。

```java
    @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\a");
        File[] files = file.listFiles();
        System.out.println(Arrays.toString(files));
    }
```

### FilenameFilter

- 我们可以FilenameFilter文件名过滤器，对文件列表进行一个过滤

```java
    @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\a");

        FilenameFilter filenameFilter = new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                return name.contains("a");
            }
        };
        
        String[] files = file.list(filenameFilter);
        System.out.println(Arrays.toString(files));
        
    }
```

```java
    @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\a");
        // for lamba
        String[] files = file.list((dir,name)->name.contains("a"));
        System.out.println(Arrays.toString(files));
    }
```

- 如果是获取文件`listFile()`方法，我们可以使用`FileFilter`,比如我们只想得到隐藏文件。

```java
    @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\a");
        FileFilter fileFilter = new FileFilter() {
            @Override
            public boolean accept(File file) {
                return file.isHidden();
            }
        };
        File[] files = file.listFiles(fileFilter);
        System.out.println(Arrays.toString(files));
    }
```

- 可以改成了方法引用的形式

```java
  @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\a");
        File[] files = file.listFiles(File::isHidden);
        System.out.println(Arrays.toString(files));
    }
```

## 3.13、mkdir() mkdirs()

- File提供了`mkdir()`创建一个目录，但是之前表示的目录必须存在。比如下面a目录必须存在才能创建d目录

```java
  @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\a\\d");
        boolean mkdir = file.mkdir();
        System.out.println(mkdir); //true
    }
```

- mkdir()不能创建一个不存在的目录下的目录，比如不存在c目录，但我仍然想在c目录下，创建d目录。就会发现创建失败了。

```java
  @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\a\\d");
        boolean mkdir = file.mkdir();
        System.out.println(mkdir); //false
    }	
```

- mkdirs()就可以创建出来一个不存在目录下的目录了，解决了这个问题。

```java
    @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\c\\d");
        boolean mkdir = file.mkdirs();
        System.out.println(mkdir); //false
    }
```

## 3.14、renameTo()

- 通过renameTo()方法，可以把一个文件名改成另一个文件。

```java
    @SneakyThrows
    @Test
    public void test(){
        File file = new File("D:\\a\\a.txt");
        boolean b = file.renameTo(new File("D:\\a\\b.txt"));
    }
```

​	
