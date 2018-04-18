## querySelector() 与 querySelectorAll()
回文档中匹配指定 CSS 选择器的一个元素
```javascript
var x = document.getElementById("mydiv");
console.log(x.querySelector(".demo"));
x.querySelector(".demo").innerHTML = "Hello World!";
x.querySelectorAll(".demo")[0].style.color = "red";

// querySelectorAll 返回的是一个 Static Node List
// getElementBy 返回的是一个 Live Node List
```

## ownerDocument 返回元素的根元素
不知道用途是什么, 猜测可能是在复杂的作用域中找到document对象(更像是在修补bug?)

