# jquery-3.3.1读源码(零)
## 自执行函数 与 调用
观察代码, 找到自执行部分为: (先把代码折叠然后一点点看)
### 1.  (function( window ){...})(window) -- 510行左右
### 2. 3895行左右 判断 if ( document.readyState === "complete" ...




## 参考(总)
### jQuery源码学习笔记系列(一) https://blog.csdn.net/cuihu811371804/article/details/77416102

## 知识点研究(总)
### 命名冲突问题