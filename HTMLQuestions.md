## HTML相关问题

#### doctype(文档类型) 的作用是什么？

声明文档类型，比如HTML或者XHTML；DOCTYPE声明告诉类似的代码校验器或者浏览器应该按照什么规则集解析文档，这些“规则”就是W3C发表的文档类型定义（DTD）中包含的规则<sup><a href="http://www.jianshu.com/p/c3dcdad42e6d">1</a></sup>。

---

#### 浏览器标准模式 (standards mode) 、几乎标准模式（almost standards mode）和怪异模式 (quirks mode) 之间的区别是什么？

由于常用Chrome，所以总是在标准模式下。

模式是浏览器渲染页面使用的规则，由DOCTYPE指定。当微软开始产生与标准兼容的浏览器时，他们希望确保向后兼容性。为了实现这一点，他们IE6.0以后的版本在浏览器内嵌了两种表现模式： Standards Mode（标准模式或Strict Mode）和Quirks mode（怪异模式或兼容模式Compatibility Mode）。在标准模式中，浏览器根据W3C所定的规范来显示页面；而在怪异模式中，页面将以IE5，甚至IE4的显示页面的方式来表现，以保持以前的网 页能正常显示。

---

#### HTML 和 XHTML 有什么区别？

XHTML是XML的子集，相比HTML语法更为严格，比如元素一定要自闭合。总结<sup><a href="https://www.sitepoint.com/differences-html-xhtml/">2</a></sup>：

**HTML:**

> - 不严格要求有起始和结束标签.
> - 只有几个空元素才要求自闭合，如` br, img, link`
> - 标签和属性不区分大小写
> - 属性可以不用引号引用
> - 一些属性值可以为空，如`checked, disabled`
> - 一些特殊字符不用转义
> - 文档需要生命HTML5 DOCTYPE

**XHTML:**

> - 所有的元素要求起始标签
> - 非空有起始标签的元素必须包含结束标签
> - 所有的元素都要自闭合
> - 标签和属性区分大小写
> - 属性必须使用双引号
> - 属性不能有空值
> - 特殊字符需要转义

---

#### 参考资料

[1] http://www.jianshu.com/p/c3dcdad42e6d

[2] https://www.sitepoint.com/differences-html-xhtml/