# snap.js 阅读
## 1. 调用与入口
```javascript
var c1 = svg1.paper.circle(50, 50, 40).attr({
    fill: "#f00"
});
```

根据上一篇 svg1 = Snap('svg1') 可知, 实际上svg1 是 snap 中的 Element.      
这句代码就是找Element.paper.circle().attr({}); 的用法 
而在paper里有 res = new Element(w); res.paper = res;
所以就相当于直接找Element.circle()

查找了一下并没有Element.circle, 进一步找 paper.circle 也没有, 编辑器自己跳到一个 Snap.plugin 里面. 在snap中定义了Snap.plugin, 在第二层自执行函数return语句前面用Snap.plugin();的方式装载了一些自定义的扩展    
而circle 就在其中的一个 Snap.plugin 中.

circle的定义及源码注释如下
```javascript
* Paper.circle
 [ method ]
 **
 * Draws a circle
 **
 - x (number) x coordinate of the centre
 - y (number) y coordinate of the centre
 - r (number) radius
 = (object) the `circle` element
 **
 > Usage
 | var c = paper.circle(50, 50, 40);
\*/
    proto.circle = function (cx, cy, r) {
        var attr;
        if (is(cx, "object") && cx == "[object Object]") {
            attr = cx;
        } else if (cx != null) {
            attr = {
                cx: cx,
                cy: cy,
                r: r
            };
        }
        return this.el("circle", attr);
    };
```

如果cx 是属性, 则默认为键值对的列表直接, 如果不是, 将cx, cy, r 做成一个键值对列表(对象), 执行的是 this.el();


## 附录
### Snap.plugin 的代码及注释
```javascript
* Snap.plugin
 [ method ]
 **
 * Let you write plugins. You pass in a function with five arguments, like this:
 | Snap.plugin(function (Snap, Element, Paper, global, Fragment) {
 |     Snap.newmethod = function () {};
 |     Element.prototype.newmethod = function () {};
 |     Paper.prototype.newmethod = function () {};
 | });
 * Inside the function you have access to all main objects (and their
 * prototypes). This allow you to extend anything you want.
 **
 - f (function) your plugin body
\*/

Snap.plugin = function (f) {
    f(Snap, Element, Paper, glob, Fragment);
};
```
