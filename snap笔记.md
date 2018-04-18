## Element
### 属性
#### node
> Gives you a reference to the DOM object, so you can assign event handlers or just mess around.    


最初是 `document.querySelector(...);` 之后添加了一些内容 
用例1. 画一个(10, 10)半径为10的圆, 点击变红 (来自snap.js的注释)
```javascript
// draw a circle at coordinate 10,10 with radius of 10
var c = paper.circle(10, 10, 10);
c.node.onclick = function () {
    c.attr("fill", "red");
}
```

#### type
> SVG tag name of the given element.   

最初是: `this.type = el.tagName || el.nodeName;`