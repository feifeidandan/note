## eval 方法族

XPathParser 提供了一系列的 `#eval*` 方法，用于获得 Boolean、Short、Integer、Long、Float、Double、String、Node 类型的元素或节点的“值”。当然，虽然方法很多，但是都是基于 `#evaluate(String expression, Object root, QName returnType)` 方法，代码如下：

```java
// XPathParser.java

/**
 * 获得指定元素或节点的值
 *
 * @param expression 表达式
 * @param root 指定节点
 * @param returnType 返回类型
 * @return 值
 */
private Object evaluate(String expression, Object root, QName returnType) {
    try {
        return xpath.evaluate(expression, root, returnType);
    } catch (Exception e) {
        throw new BuilderException("Error evaluating XPath.  Cause: " + e, e);
    }
}
```

调用 `xpath` 的 `evaluate(String expression, Object root, QName returnType)` 方法，获得指定元素或节点的值