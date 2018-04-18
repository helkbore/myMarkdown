# js: tagName 与 nodeName 的区别

DOM 节点类型有12种
常见的有:
元素节点, 属性节点, 文本节点    

nodeName 返回节点名 (node 接口)     
tagName 返回元素的标签名 (element 接口)     
所有节点继承 node, 元素节点还继承了 element(element 继承自 node 及其扩展接口 EventTarget)     
同时作用在元素节点时效果一样   


## 附
### 1. 12中类型
元素节点            　　Node.ELEMENT_NODE(1)
属性节点            　　Node.ATTRIBUTE_NODE(2)
文本节点            　　Node.TEXT_NODE(3)
CDATA节点             Node.CDATA_SECTION_NODE(4)
实体引用名称节点    　　 Node.ENTRY_REFERENCE_NODE(5)
实体名称节点        　　Node.ENTITY_NODE(6)
处理指令节点        　　Node.PROCESSING_INSTRUCTION_NODE(7)
注释节点            　 Node.COMMENT_NODE(8)
文档节点            　 Node.DOCUMENT_NODE(9)
文档类型节点        　　Node.DOCUMENT_TYPE_NODE(10)
文档片段节点        　　Node.DOCUMENT_FRAGMENT_NODE(11)
DTD声明节点            Node.NOTATION_NODE(12)


###  关于node 接口
nodeName 节点名称
nodeValue 节点值(字符串)

### 关于元素节点
节点类型nodeType 值为 1
nodeName: 大写标签名
nodeValue: null

### 参考与感谢
https://blog.csdn.net/borishuai/article/details/5719227
http://www.cnblogs.com/xiaohuochai/p/5785189.html