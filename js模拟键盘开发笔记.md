# js模拟键盘开发笔记
## (一). 实现方式
### 一. 页面显示
#### 整体: 一个 `ul` 列表. 每个 `li` 是键盘上一个键
#### 键盘分类-三种: 
1. 可以 `shift` 的(如 `1` 和 `!` 等数字符号键)
2. 字母键
3. 功能键(回车, 空格, 退格, `capslock` 等)

#### 键盘布局方式
可以使用浮动或 `flex`, 设定总体宽度, 然后单独设定特殊长度的键宽度, 元素自动换行.

```xml
<ul id="keyboard">
    <li class="symbol"><span class="off">1</span><span class="on">!</span></li>
    <li class="delete lastitem">delete</li>
    <li class="tab">tab</li>
    <li class="letter">q</li>
    <li class="capslock">caps lock</li>
    <li class="return lastitem">return</li>
    <li class="left-shift">shift</li>
    <li class="right-shift lastitem">shift</li>
    <li class="space lastitem">&nbsp;</li>
</ul>
```

### 二. js 功能
js 功能分为: 
+ 设置事件监听(键盘点击, 文本框焦点)
+ 特殊功能键的实现: `CapsLock` , `Shift` - 恢复 `Shift` , `delete` (退格), `Space` (空格)
+ 字母-大写字母, 数字-特殊符号向表单文本框回写
#### 1. `Caps Lock` 功能实现
css 字母键增加一个大写类
```css
.uppercase {text-transform: uppercase;}
```
js 切换
```javascript
$('.letter').toggleClass('uppercase');
```
#### 2. `shift` 键功能实现  
+ 实现大写功能-键盘显示大写字母(见上一条)
+ 实现上档功能-键盘显示数字键代表的符号等  

html代码:   
```xhtml
<li class="symbol"><span class="off">1</span><span class="on">!</span></li>
```

css 代码: 
```css
.on {display: none;}
```

```javascript
$('.symbol span').toggle();
```
补充说明:
`toggle()` 是 `jQuery`函数, 切换显示和隐藏.

#### 3. `delete` (退格) 实现
+ 获取文本框内容
+ substr截取: `html.substr(0, html.length - 1)`
+ 回写文本框

#### 4. 特殊字符
标点, 空格, 回车, 制表符   
标点: `character = $('span:visible', $this).html();`  
回车/制表/空格: `character = "\n";`   


## (二). 笔记
### 1. 焦点事件  
两个文本框, 想判断当前焦点是哪个  
+ 事件对象: `document.activeElement`  
+ 监听文本框失去焦点和获取焦点事件, 判断  

代码:   
```javascript
var current_focus_id = document.activeElement.id;
if (current_focus_id && current_focus_id.toUpperCase() == 'USERNAME' || current_focus_id.toUpperCase() == 'PASSWORD'){
    input_element_id = current_focus_id;
}

```
遇到问题: 发现无法获取当前焦点, 猜测:  
A失去焦点, B获得焦点. 两个动作是同时发生的, 但监听A失去焦点时, B并未获得焦点
解决办法:  
添加定时器, 延期0秒执行(则B获得焦点, 代码如下: )  
```javascript
 setTimeout(function(){
    var current_focus_id = document.activeElement.id;
    if (current_focus_id && current_focus_id.toUpperCase() == 'USERNAME' || current_focus_id.toUpperCase() == 'PASSWORD'){
        input_element_id = current_focus_id;
    }
}, 0);
``` 

### 2. 用`jquery`的`delegate`添加事件
> 指定的元素（属于被选元素的子元素）添加一个或多个事件处理程序，并规定当这些事件发生时运行的函数。   
> 使用 delegate() 方法的事件处理程序适用于当前或未来的元素（比如由脚本创建的新元素）。  

代码:   
```javascript
$('.login').delegate("*", "focus blur", function (e) {
     setTimeout(function(){
        var current_focus_id = document.activeElement.id;
        if (current_focus_id && current_focus_id.toUpperCase() == 'USERNAME' || current_focus_id.toUpperCase() == 'PASSWORD'){
            input_element_id = current_focus_id;
        }
    }, 0);
});
```

猜测实现:    
+ 遍历指定元素的所有符合选择器的元素, 
+ 为他们添加事件监听
+ 绑定事件触发时执行的函数

### 3. css 样式设置为: `text-transform: uppercase` 时, 显示的值为大写, 但实际值(如 innerHTML / value ) 仍为小写