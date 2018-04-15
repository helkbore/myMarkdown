# js: 为元素添加trigger方法

##` []['forEach'].call(..){....}`理解

### `[]`的含义
> []是一个数组，这个数组根本不用，它被放在页面上，因为使用数组可以访问数组原型，例如.forEach。

> 这比打字更快 Array.prototype.forEach.call(...)；

### forEach
> forEach是将函数作为输入的函数...

  `
> forEach() 方法用于调用数组的每个元素，并将元素传递给回调函数。
注意: forEach() 对于空数组是不会执行回调函数的。

### call()
> 官方说法是: 调用一个对象的一个方法，以另一个对象替换当前对象。

比较难懂, 推荐看[博客地址](http://blog.csdn.net/longjuesi/article/details/29859599)来理解

参考:
>http://www.runoob.com/jsref/jsref-foreach.html  
>https://www.cnblogs.com/yanxiaoluo/p/7605850.html  
>http://blog.csdn.net/ywl570717586/article/details/52681392  
>http://blog.csdn.net/longjuesi/article/details/29859599 


代码地址: [https://github.com/limingziqiang/functions/blob/master/ElementTrigger.js](https://github.com/limingziqiang/functions/blob/master/ElementTrigger.js) 