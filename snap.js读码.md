# snap.js 阅读
## 0. 大致结构: 两个自执行函数
看了注释后一个是eve(javascript 事件库), 一个是snap

## 1. 入口
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
