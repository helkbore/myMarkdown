# js 读代码的一些摘抄

## 生成id
```javascript
idgen = 0,
idprefix = "S" + (+new Date).toString(36),
ID = function (el) {
    return (el && el.type ? el.type : E) + idprefix + (idgen++).toString(36);
}
```