# js笔记
## 1. Object.prototype.hasOwnProperty()
hasOwnProperty() 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性
```javascript
var a = [1,2,3,4,5,6];
console.log(a["hasOwnProperty"](5));
```
正常用法是a.hasOwnProperty(5) 但是 属性可以用数组下标的方式表示(甚至更全面?), 所以才会有上面的用法

## 2. unshift()
向数组的开头添加一个或更多元素，并返回新的长度。

## 3. document.onreadystatechange 页面加载状态改变时的事件
## 4. document.readyState 返回当前文档的状态