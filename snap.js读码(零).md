# snap.js读码(零) -- 自执行(预加载)

## 一. Snap.format = (function () {
## 二. eve.on("snap.util.getattr", function () {
### 1. eve.on()
参数是一个字符串和一个函数
```javascript
comaseparator = /\s*,\s*/;
Str = String;
objtos = Object.prototype.toString;
separator = /[\.\/]/;
events = {n: {}};

isArray = Array.isArray || function (ar) {
    return ar instanceof Array || objtos.call(ar) == "[object Array]";
};

eve.on = function (name, f) {
    if (typeof f != "function") {
        return function () {};
    }
    var names = isArray(name) ? (isArray(name[0]) ? name : [name]) : Str(name).split(comaseparator);
    console.log(names)
    for (var i = 0, ii = names.length; i < ii; i++) {
        (function (name) {
            var names = isArray(name) ? name : Str(name).split(separator),
                e = events,
                exist;
            for (var i = 0, ii = names.length; i < ii; i++) {
                e = e.n;
                e = e.hasOwnProperty(names[i]) && e[names[i]] || (e[names[i]] = {n: {}});
            }
            e.f = e.f || [];
            for (i = 0, ii = e.f.length; i < ii; i++) if (e.f[i] == f) {
                exist = true;
                break;
            }
            !exist && e.f.push(f);
        }(names[i]));
    }
    return function (zIndex) {
        if (+zIndex == +zIndex) {
            f.zIndex = +zIndex;
        }
    };
};
```