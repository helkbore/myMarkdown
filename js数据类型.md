## isNaN
```javascript
console.log(isNaN(NaN));    //NaN就是Not-A-Number
console.log(isNaN(undefined));//undefined什么都不是，当然也Not-A-Number.
console.log(isNaN(null));// 能转成0
console.log(isNaN(""));  // 能转成0
console.log(isNaN({}));  // 相当于undefined
console.log(isNaN([]));  // 能转成0
console.log(isNaN(new Object()));  //NaN
console.log(isNaN(new String()));  //能转成0
console.log(isNaN(new String("a"))); //转成字符串
console.log(isNaN(new Array()));  //能转成0
console.log(isNaN(new Date()));  //能转成数字
console.log(isNaN(new Date().toString()));  //转成字符串
console.log(isNaN(true));//能转成1
console.log(isNaN(0/0)); //结果就是NaN
```

## isFinite
## 轮子, 判断对象类型
最后一句似懂非懂啊
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
		Object.prototype.toString.call(o).slice(8, -1).toLowerCase() == type;
}
```