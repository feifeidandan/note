# XPathParser构造方法

XPathParser 的构造方法有 16 个之多，当然基本都非常相似，我们来挑选其中一个。代码如下：

```java
// XPathParser.java

/**
 * 构造 XPathParser 对象
 *
 * @param xml XML 文件地址
 * @param validation 是否校验 XML
 * @param variables 变量 Properties 对象
 * @param entityResolver XML 实体解析器
 */
public XPathParser(String xml, boolean validation, Properties variables, EntityResolver entityResolver) {
    commonConstructor(validation, variables, entityResolver);
    this.document = createDocument(new InputSource(new StringReader(xml)));
}
```

调用 `#commonConstructor(boolean validation, Properties variables, EntityResolver entityResolver)` 方法，公用的构造方法逻辑。代码如下：

```java
// XPathParser.java

private void commonConstructor(boolean validation, Properties variables, EntityResolver entityResolver) {
    this.validation = validation;
    this.entityResolver = entityResolver;
    this.variables = variables;
    // 创建 XPathFactory 对象
    XPathFactory factory = XPathFactory.newInstance();
    this.xpath = factory.newXPath();
}
```

调用 `#createDocument(InputSource inputSource)` 方法，将 XML 文件解析成 Document 对象。代码如下：

```java
// XPathParser.java

/**
 * 创建 Document 对象
 *
 * @param inputSource XML 的 InputSource 对象
 * @return Document 对象
 */
private Document createDocument(InputSource inputSource) {
    // important: this must only be called AFTER common constructor
    try {
        // 1> 创建 DocumentBuilderFactory 对象
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        factory.setValidating(validation); // 设置是否验证 XML

        factory.setNamespaceAware(false);
        factory.setIgnoringComments(true);
        factory.setIgnoringElementContentWhitespace(false);
        factory.setCoalescing(false);
        factory.setExpandEntityReferences(true);

        // 2> 创建 DocumentBuilder 对象
        DocumentBuilder builder = factory.newDocumentBuilder();
        builder.setEntityResolver(entityResolver); // 设置实体解析器
        builder.setErrorHandler(new ErrorHandler() { // 实现都空的

            @Override
            public void error(SAXParseException exception) throws SAXException {
                throw exception;
            }

            @Override
            public void fatalError(SAXParseException exception) throws SAXException {
                throw exception;
            }

            @Override
            public void warning(SAXParseException exception) throws SAXException {
            }

        });
        // 3> 解析 XML 文件
        return builder.parse(inputSource);
    } catch (Exception e) {
        throw new BuilderException("Error creating document instance.  Cause: " + e, e);
    }
}
```

就是简单的 Java XML API 的使用。