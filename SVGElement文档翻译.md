# SVGElement文档翻译
来自: https://developer.mozilla.org/en-US/docs/Web/API/SVGElement

> All of the SVG DOM interfaces that correspond directly to elements in the SVG language derive from the SVGElement interface.
所有SVG元素的SVG DOM 接口都来自 SVGElement 接口

## 属性
### SVGElement.dataset (只读)
> A DOMStringMap object which provides a list of key/value pairs of named data attributes which correspond to custom data attributes attached to the element. These can also be defined in SVG using attributes of the form data-*, where * is the key name for the pair. This works just like HTML's HTMLElement.dataset property and HTML's data-* global attribute.    

"DOMStringMap"对象, 是元素自定义属性( custom data attributes )的键值对列表 .在 SVG 中通过使用SVGAttr("data-*")属性, * 是键名. 与HTML的 "HTMLElement.dataset" 属性以及 HTML的 data-* 全局属性类似

### SVGElement.id (只读)
> A DOMString representing the value of the id attribute on the given element, or the empty string if id is not present.    
  
DOMString, 表示一个元素的 id 属性的值. 若 id 不存在, 则该值为空


### SVGElement.xmlbase (只读)
> A DOMString corresponding to the xml:base attribute on the given element.     
    
DOMString, 与xml:base属性相对应.

### SVGElement.ownerSVGElement (只读)
> An SVGSVGElement referring to the nearest ancestor <svg> element. null if the given element is the outermost <svg> element.    

SVGSVGElement, 指向最近的<svg>祖先元素, 如果给定元素是最外层的<svg>, 则返回null

### SVGElement.viewportElement (只读)
> The SVGElement, which established the current viewport. Often, the nearest ancestor <svg> element. null if the given element is the outermost <svg> element.     

SVGElement, 当前视窗(viewport) 建立的 SVGElement, 通常是最近的<svg>祖先元素, 如果给定元素是最外层的<svg>, 则返回null


## 方法
> The SVGElement interface doesn't provide any additional methods, but inherits methods from its parent, Element.   

SVGElement 接口没有提供额外方法, 只继承自 Element 的方法