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


## 2. Paper.prototype.el
从circle接收到的参数,传给了el, 使得能够通过paper.circle调用(所以 circle实际的工作就是整理了一下attr)   


```javascript
Paper.prototype.el = function (name, attr) {
    var el = make(name, this.node);
    attr && el.attr(attr);
    return el;
};
```

## 3. make
```javascript
function make(name, parent) {
    var res = $(name);
    parent.appendChild(res);
    var el = wrap(res);
    return el;
}
```

这里用到了$()根据笔记(一)知道实现的是 `document.createElementNS(xmlns, el)`

所以 `make` 翻译一下就是
```javascript
function make(name, parent) {
    var res = document.createElementNS(xmlns, name);
    parent.appendChild(res);
    var el = wrap(res);
    return el;
    
}
```

继续往上 到Paper.prototype.el 的话就是
```javascript
Paper.prototype.el = function (name, attr) {
    var res = document.createElementNS(xmlns, name);
    this.node.appendChild(res);
    var el = wrap(res);
    attr && el.attr(attr);
    return el;
};
```

最后到顶层的circle定义时是: 
```javascript
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
    
    var res = document.createElementNS(xmlns, "circle");
    this.node.appendChild(res);
    var el = wrap(res);
    attr && el.attr(attr);
    return el;
};
```

那么入口为 
`var c0 = svg1.circle(50, 50, 40).attr({fill: "#ff0"});`
或者
`var c1 = svg1.paper.circle(50, 50, 40).attr({fill: "#f00"});`
的代码的历程就只剩下 wrap(res) 和 attr() 这两步没有解决了(wrap文档(一)里面看过了, 但这次看是不是走了其他分支)

## 4. wrap()
回顾一下~ 我们文档(一)里是Snap('#svg1');传到wrap的时候, 走了第三个分支即
`if (dom.tagName && dom.tagName.toLowerCase() == "svg") {`
那么这一次呢? 我们的参数即wrap中的dom 是 res, 即 `var res = document.createElementNS(xmlns, "circle");` 所以结论应该是不走判断,直接返回


```javascript
function wrap(dom) {
    if (!dom) {
        return dom;
    }
    if (dom instanceof Element || dom instanceof Fragment) {
        return dom;
    }
    if (dom.tagName && dom.tagName.toLowerCase() == "svg") {
        return new Paper(dom);
    }
    if (dom.tagName && dom.tagName.toLowerCase() == "object" && dom.type == "image/svg+xml") {
        return new Paper(dom.contentDocument.getElementsByTagName("svg")[0]);
    }
    return new Element(dom);
}
```
经过了wrap, 将整个路径的代码再次合并的话是: 
```javascript
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
    
    var res = document.createElementNS(xmlns, "circle");
    this.node.appendChild(res);
    var el = new Element(res);
    attr && el.attr(attr);
    return el;
};
```

element在文档一也看过了, 这次看看是否也会有不同

## 5 Element
```javascript
function Element(el) {
    var svg;
    try {
        svg = el.ownerSVGElement;
    } catch(e) {}

    this.node = el;
    if (svg) {
        this.paper = new Paper(svg);
    }

    this.type = el.tagName || el.nodeName;
    var id = this.id = ID(this);
    this.anims = {};
    this._ = {
        transform: []
    };
    el.snap = id;
    hub[id] = this;
}
```
发现也是生成了一个snap特色的dom对象, 装配了一些属性, 并注册进一些全局变量, 具体的是: (这次具体一下好了)
如果是svg元素的话
node: 原生DOM--至少这里初始化的时候是
type: 元素的标签名
id: 随机生成的唯一id
anims: 空对象,目前不知道用途
_: 里面有一个属性是`transform: []`

对其他对象的影响: 
el.snap = id (将id 赋给了传进来的参数, 即原生的DOM)
hub[id] = this; 把这个新初始化的element对象注册到了hub数组中

如果是隶属于svg但不是`g, mask, pattern, symbol`的话
同上多了个this.paper = 顶层svg元素(DOM原生)

所以element作用就是完成了snap特色的dom元素
到此为止, 进入下一步 attr


## 6 attr()
看上面的整合代码可知, el是一个element对象, 所以要找的是element.attr
传进来的参数是这样的: `el.attr(attr)` (attr 是一个对象, 记录了圆的顶点坐标和半径)
```javascript
var has = "hasOwnProperty";
Element.prototype.attr = function (params, value) {
    var el = this,
        node = el.node;
    if (!params) {...}
    if (is(params, "string")){...}
    for (var att in params) {
        if (params[has](att)) {
            eve("snap.util.attr." + att, el, params[att]);
        }
    }
    return el;
};
```

简单来说就是遍历attr对象然后每个属性都调用
`eve("snap.util.attr." + att, el, params[att]);`
所以下一步是eve

## 7. eve
```javascript
eve = function (name, scope) {
    var e = events,
        oldstop = stop,
        args = Array.prototype.slice.call(arguments, 2),
        listeners = eve.listeners(name),
        z = 0,
        f = false,
        l,
        indexed = [],
        queue = {},
        out = [],
        ce = current_event,
        errors = [];
    out.firstDefined = firstDefined;
    out.lastDefined = lastDefined;
    current_event = name;
    stop = 0;
    for (var i = 0, ii = listeners.length; i < ii; i++) if ("zIndex" in listeners[i]) {
        indexed.push(listeners[i].zIndex);
        if (listeners[i].zIndex < 0) {
            queue[listeners[i].zIndex] = listeners[i];
        }
    }
    indexed.sort(numsort);
    while (indexed[z] < 0) {
        l = queue[indexed[z++]];
        out.push(l.apply(scope, args));
        if (stop) {
            stop = oldstop;
            return out;
        }
    }
    for (i = 0; i < ii; i++) {
        l = listeners[i];
        if ("zIndex" in l) {
            if (l.zIndex == indexed[z]) {
                out.push(l.apply(scope, args));
                if (stop) {
                    break;
                }
                do {
                    z++;
                    l = queue[indexed[z]];
                    l && out.push(l.apply(scope, args));
                    if (stop) {
                        break;
                    }
                } while (l)
            } else {
                queue[l.zIndex] = l;
            }
        } else {
            out.push(l.apply(scope, args));
            if (stop) {
                break;
            }
        }
    }
    stop = oldstop;
    current_event = ce;
    return out;
};
```
前面一堆变量赋值, 到循环`for (var i = 0, ii = listeners.length; i < ii; i++)`
需要看一下listeners是什么, 即 `eve.listeners(name)` 其中name 值是: `snap.util.attr.cx`




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

### `eve.listeners(name)` 其中name 值是: `snap.util.attr.cx`
```javascript
var separator = /[\.\/]/;
eve.listeners = function (name) {
    var names = isArray(name) ? name : name.split(separator),
        e = events,
        item,
        items,
        k,
        i,
        ii,
        j,
        jj,
        nes,
        es = [e],
        out = [];
    for (i = 0, ii = names.length; i < ii; i++) {
        nes = [];
        for (j = 0, jj = es.length; j < jj; j++) {
            e = es[j].n;
            items = [e[names[i]], e[wildcard]];
            k = 2;
            while (k--) {
                item = items[k];
                if (item) {
                    nes.push(item);
                    out = out.concat(item.f || []);
                }
            }
        }
        es = nes;
    }
    return out;
};
```