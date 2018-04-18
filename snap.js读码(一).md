# snap.js 阅读
## Snap("#svg1")
### 0. 大致结构: 两个自执行函数
看了注释后一个是eve(javascript 事件库), 一个是snap

### 1. 入口
var svg1 = Snap('#svg1');

观察代码, 简化后运行的顺序应该是:
```javascript
(function(glob, factory){
    factory(glob);
})(window || this, function(window) {
    var Snap = (function(root){
        function Snap(){
            console.log('success');
        }
        var glob = {
            win: root.window,
            doc: root.window.document
        };

        glob.win.Snap = Snap;

        return Snap;
    })(window || this);

    return Snap
});

    Snap();
```

### 2. snap()
所以程序应该是直接进入到了
```javascript
function Snap(w, h) {
    if (w) {
        if (w.nodeType) {
            return wrap(w);
        }
        if (is(w, "array") && Snap.set) {
            return Snap.set.apply(Snap, w);
        }
        if (w instanceof Element) {
            return w;
        }
        if (h == null) {
            try {
                w = glob.doc.querySelector(String(w));
                return wrap(w);
            } catch (e) {
                return null;
            }
        }
    }
    w = w == null ? "100%" : w;
    h = h == null ? "100%" : h;
    return new Paper(w, h);
}
```
由于我传入的是"#svg1", 所以直接执行的是
```javascript
 w = glob.doc.querySelector(String(w));
// w = window.document.querySelector(String('#svg1'));
 return wrap(w);
```

### 3. wrap
找到wrap定义
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

在wrap里分类了一下, 我们的程序走的是'svg'
所以直接调用了 `new Paper(dom);`

目前为止程序实质上比调用时执行了: 
```javascript
w = window.document.querySelector(String('#svg1'));
```

### 4. paper(); 参数: w - '#svg1', h - 未传入
```javascript
function Paper(w, h) {
    var res,
        desc,
        defs,
        proto = Paper.prototype;
    if (w && w.tagName && w.tagName.toLowerCase() == "svg") {
        if (w.snap in hub) {
            return hub[w.snap];
        }
        var doc = w.ownerDocument;
        res = new Element(w);
        desc = w.getElementsByTagName("desc")[0];
        defs = w.getElementsByTagName("defs")[0];
        if (!desc) {
            desc = $("desc");
            desc.appendChild(doc.createTextNode("Created with Snap"));
            res.node.appendChild(desc);
        }
        if (!defs) {
            defs = $("defs");
            res.node.appendChild(defs);
        }
        res.defs = defs;
        for (var key in proto) if (proto[has](key)) {
            res[key] = proto[key];
        }
        res.paper = res.root = res;
    } else {
        res = make("svg", glob.doc.body);
        $(res.node, {
            height: h,
            version: 1.1,
            width: w,
            xmlns: xmlns
        });
    }
    return res;
}
```

粗略上看: 进选择(属于svg元素),   执行 `res = new Element(w);` 后面判断修改了一下(判断是否有defs和desc, 如果没有获取该元素- [用getElementsByTagName 和 appendChild 的原生js] -) 就加一个新的(这里的$('desc') 先暂时不研究, 后面再看), 最后返回 res
所以好像就差最后一步的样子, 即: `new Element(w)`; 

注意 这里有代码, 定义了res 的一些属性 如 'key'指向(paper原型自带的属性 这个时候的paper的原型还是object), paper, root 都是指向本身, 
```javascript
for (var key in proto) if (proto[has](key)) {
    res[key] = proto[key];
}
res.paper = res.root = res;
```

所以到这一步(已经看了$("defs")的含义, 所以代码执行的步骤可以简化为)
```javascript
w = document.querySelector(String('#svg1'));
res = new Element(w);

// 添加 defs 和 desc
xmlns = "http://www.w3.org/2000/svg";
defs = document.createElementNS(xmlns, "defs");
desc = document.createElementNS(xmlns, "desc");
desc.appendChild(doc.createTextNode("Created with Snap"));
res.node.appendChild(desc);
res.node.appendChild(defs);

return res;
```

### 5. new Element(w) 参数: w 为 '#svg1'
只摘取此种调用情况涉及的代码
```javascript
function Element(el) {
    this.node = el;   
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

以上就是一个普通的<svg>标签初始化的过程. 将 snap 的 Element 与 dom 关联上, 并做了一些属性和全局变量的注册 (后面应该还会有定义方法和属性, 这样就可以直接使用了)

## 总结
Snap('#svg1')执行的顺序大致是 
> snap() -> wrap() -> paper() -> element()

## 其他
### 1. $()
调用范例:  
`$("desc");`
`$("defs");`

```javascript
xmlns = "http://www.w3.org/2000/svg";

function $(el, attr) {
    if (attr) {...} else {
        el = glob.doc.createElementNS(xmlns, el);
    }
    return el;
}
```

单独传参的话, 调用的是`document.createElementNS(xmlns, el)`;


### is()
```javascript
function is(o, type) {
    type = Str.prototype.toLowerCase.call(type);
    if (type == "finite") {
        return isFinite(o);
    }
    if (type == "array" &&
        (o instanceof Array || Array.isArray && Array.isArray(o))) {
        return true;
    }
    return  type == "null" && o === null ||
        type == typeof o && o !== null ||
        type == "object" && o === Object(o) ||
        objectToString.call(o).slice(8, -1).toLowerCase() == type;
}
```