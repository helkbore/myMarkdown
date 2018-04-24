# js 读代码的一些摘抄

## 生成id
```javascript
idgen = 0,
idprefix = "S" + (+new Date).toString(36),
ID = function (el) {
    return (el && el.type ? el.type : E) + idprefix + (idgen++).toString(36);
}
```

## 简单短路
如果变量a 存在 则使用a
```javascript
attr && el.attr(attr)
```

## isArray
```javascript
isArray = Array.isArray || function (ar) {
    return ar instanceof Array || objtos.call(ar) == "[object Array]";
};
```