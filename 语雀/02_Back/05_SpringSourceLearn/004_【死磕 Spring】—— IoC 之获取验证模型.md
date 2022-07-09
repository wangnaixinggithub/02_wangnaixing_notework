# 【死磕 Spring】—— IoC 之获取验证模型

在上篇博客[【死磕 Spring】—— IoC 之加载 Definitions](http://svip.iocoder.cn/Spring/IoC-load-BeanDefinitions) 中提到，在核心逻辑方法 `#doLoadBeanDefinitions(InputSource inputSource, Resource resource)` 方法中，中主要是做三件事情：

1. 调用 `#getValidationModeForResource(Resource resource)` 方法，获取指定资源（xml）的**验证模式**。
2. 调用 `DocumentLoader#loadDocument(InputSource inputSource, EntityResolver entityResolver,ErrorHandler errorHandler, int validationMode, boolean namespaceAware)` 方法，获取 XML Document 实例。
3. 调用 `#registerBeanDefinitions(Document doc, Resource resource)` 方法，根据获取的 Document 实例，注册 Bean 信息。

这篇博客主要**第 1 步**，分析获取 xml 文件的验证模式。为什么需要获取验证模式呢？原因如下：

> XML 文件的验证模式保证了 XML 文件的正确性。

# 1. DTD 与 XSD 的区别

## 1.1 DTD

DTD(Document Type Definition)，即文档类型定义，为 XML 文件的验证机制，属于 XML 文件中组成的一部分。DTD 是一种保证 XML 文档格式正确的有效验证方式，它定义了相关 XML 文档的元素、属性、排列方式、元素的内容类型以及元素的层次结构。其实 DTD 就相当于 XML 中的 “词汇”和“语法”，我们可以通过比较 XML 文件和 DTD 文件 来看文档是否符合规范，元素和标签使用是否正确。

要在 Spring 中使用 DTD，需要在 Spring XML 文件头部声明：

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC  "-//SPRING//DTD BEAN//EN"  "http://www.springframework.org/dtd/spring-beans.dtd">
```

DTD 在一定的阶段推动了 XML 的发展，但是它本身存在着一些**缺陷**：

1. 它没有使用 XML 格式，而是自己定义了一套格式，相对解析器的重用性较差；而且 DTD 的构建和访问没有标准的编程接口，因而解析器很难简单的解析 DTD 文档。
2. DTD 对元素的类型限制较少；同时其他的约束力也叫弱。
3. DTD 扩展能力较差。
4. 基于正则表达式的 DTD 文档的描述能力有限。

## 1.2 XSD

针对 DTD 的缺陷，W3C 在 2001 年推出 XSD。XSD（XML Schemas Definition）即 XML Schema 语言。XML Schema 本身就是一个 XML文档，使用的是 XML 语法，因此可以很方便的解析 XSD 文档。相对于 DTD，XSD 具有如下**优势**：

1. XML Schema 基于 XML ，没有专门的语法。
2. XML Schema 可以象其他 XML 文件一样解析和处理。
3. XML Schema 比 DTD 提供了更丰富的数据类型。
4. XML Schema 提供可扩充的数据模型。
5. XML Schema 支持综合命名空间。
6. XML Schema 支持属性组。

# 2. getValidationModeForResource

```java
// XmlBeanDefinitionReader.java

// 禁用验证模式
public static final int VALIDATION_NONE = XmlValidationModeDetector.VALIDATION_NONE;
// 自动获取验证模式
public static final int VALIDATION_AUTO = XmlValidationModeDetector.VALIDATION_AUTO;
// DTD 验证模式
public static final int VALIDATION_DTD = XmlValidationModeDetector.VALIDATION_DTD;
// XSD 验证模式
public static final int VALIDATION_XSD = XmlValidationModeDetector.VALIDATION_XSD;
	
/**
 * 验证模式。默认为自动模式。
 */
private int validationMode = VALIDATION_AUTO;
	
protected int getValidationModeForResource(Resource resource) {
	// <1> 获取指定的验证模式
	int validationModeToUse = getValidationMode();
	// 首先，如果手动指定，则直接返回
	if (validationModeToUse != VALIDATION_AUTO) {
		return validationModeToUse;
	}
	// 其次，自动获取验证模式
	int detectedMode = detectValidationMode(resource);
	if (detectedMode != VALIDATION_AUTO) {
		return detectedMode;
	}
	// 最后，使用 VALIDATION_XSD 做为默认
	// Hmm, we didn't get a clear indication... Let's assume XSD,
	// since apparently no DTD declaration has been found up until
	// detection stopped (before finding the document's root tag).
	return VALIDATION_XSD;
}

```

`<1>` 处，调用 `#getValidationMode()` 方法，获取指定的验证模式( `validationMode` )。如果有手动指定，则直接返回。另外，对对于 `validationMode` 属性的设置和获得的代码，代码如下：

```java
public void setValidationMode(int validationMode) {
	this.validationMode = validationMode;
}

public int getValidationMode() {
	return this.validationMode;
}
```

`<2>` 处，调用 `#detectValidationMode(Resource resource)` 方法，自动获取验证模式。代码如下：

```java
/**
   * XML 验证模式探测器
   */
  private final XmlValidationModeDetector validationModeDetector = new XmlValidationModeDetector();
	
  protected int detectValidationMode(Resource resource) {
// 不可读，抛出 BeanDefinitionStoreException 异常
  	if (resource.isOpen()) {
  		throw new BeanDefinitionStoreException(
  				"Passed-in Resource [" + resource + "] contains an open stream: " +
  				"cannot determine validation mode automatically. Either pass in a Resource " +
  				"that is able to create fresh streams, or explicitly specify the validationMode " +
  				"on your XmlBeanDefinitionReader instance.");
  	}
  
  	// 打开 InputStream 流
  	InputStream inputStream;
  	try {
  		inputStream = resource.getInputStream();
  	} catch (IOException ex) {
  		throw new BeanDefinitionStoreException(
  				"Unable to determine validation mode for [" + resource + "]: cannot open InputStream. " +
  				"Did you attempt to load directly from a SAX InputSource without specifying the " +
  				"validationMode on your XmlBeanDefinitionReader instance?", ex);
  	}
  
  	// <x> 获取相应的验证模式
  	try {
  		return this.validationModeDetector.detectValidationMode(inputStream);
  	} catch (IOException ex) {
  		throw new BeanDefinitionStoreException("Unable to determine validation mode for [" +
  				resource + "]: an error occurred whilst reading from the InputStream.", ex);
  	}
  }
```

- - 核心在于 `<x>` 处，调用 `XmlValidationModeDetector#detectValidationMode(InputStream inputStream)` 方法，获取相应的验证模式。详细解析，见 [「3. XmlValidationModeDetector」](http://svip.iocoder.cn/Spring/IoC-Validation-Mode-For-Resource/#) 中。
- `<3>` 处，使用 `VALIDATION_XSD` 做为默认。

# 3. XmlValidationModeDetector

`org.springframework.util.xml.XmlValidationModeDetector` ，XML 验证模式探测器。

```java
public int detectValidationMode(InputStream inputStream) throws IOException {
    // Peek into the file to look for DOCTYPE.
    BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
    try {
        // 是否为 DTD 校验模式。默认为，非 DTD 模式，即 XSD 模式
        boolean isDtdValidated = false;
        String content;
        // <0> 循环，逐行读取 XML 文件的内容
        while ((content = reader.readLine()) != null) {
            content = consumeCommentTokens(content);
            // 跳过，如果是注释，或者
            if (this.inComment || !StringUtils.hasText(content)) {
                continue;
            }
            // <1> 包含 DOCTYPE 为 DTD 模式
            if (hasDoctype(content)) {
                isDtdValidated = true;
                break;
            }
            // <2>  hasOpeningTag 方法会校验，如果这一行有 < ，并且 < 后面跟着的是字母，则返回 true 。
            if (hasOpeningTag(content)) {
                // End of meaningful data...
                break;
            }
        }
        // 返回 VALIDATION_DTD or VALIDATION_XSD 模式
        return (isDtdValidated ? VALIDATION_DTD : VALIDATION_XSD);
    } catch (CharConversionException ex) {
           
        // <3> 返回 VALIDATION_AUTO 模式
        // Choked on some character encoding...
        // Leave the decision up to the caller.
        return VALIDATION_AUTO;
    } finally {
        reader.close();
    }
}
```

- `<0`> 处，从代码中看，主要是通过读取 XML 文件的内容，来进行自动判断。
- `<1>` 处，调用 `#hasDoctype(String content)` 方法，判断内容中如果包含有 `"DOCTYPE`“ ，则为 DTD 验证模式。代码如下：

```java
/**
 * The token in a XML document that declares the DTD to use for validation
 * and thus that DTD validation is being used.
 */
private static final String DOCTYPE = "DOCTYPE";

private boolean hasDoctype(String content) {
	return content.contains(DOCTYPE);
}
```

`<2>` 处，调用 `#hasOpeningTag(String content)` 方法，判断如果这一行包含 `<` ，并且 `<` 紧跟着的是字幕，则为 XSD 验证模式。代码如下：

```java
**
 * Does the supplied content contain an XML opening tag. If the parse state is currently
 * in an XML comment then this method always returns false. It is expected that all comment
 * tokens will have consumed for the supplied content before passing the remainder to this method.
 */
private boolean hasOpeningTag(String content) {
	if (this.inComment) {
		return false;
	}
	int openTagIndex = content.indexOf('<');
	return (openTagIndex > -1 // < 存在
            && (content.length() > openTagIndex + 1) // < 后面还有内容
            && Character.isLetter(content.charAt(openTagIndex + 1))); // < 后面的内容是字幕
}
```

- `<3>` 处，如果发生 CharConversionException 异常，则为 `VALIDATION_AUTO` 模式。

关于 `#consumeCommentTokens(String content)` 方法，代码比较复杂。感兴趣的胖友可以看看。





