# jquery-3.3.1读源码(壹)

## 1. isFunction
有些浏览器 对dom节点使用 typeof 的结果会是 function

```javascript
return typeof obj === "function" && typeof obj.nodeType !== "number";
```
## 2. isWindow
window对象是一个包含自己的对象
```javascript
obj != null && obj === obj.window;
```
> 真正用的时候前面要排除一些情况的, 比如自己定义了window属性的

## 3. DOMEval  DOM中动态插入并执行脚本

## 4. toType


