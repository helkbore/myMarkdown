## 1. line-height: 1.15
设置数字，此数字会与当前的字体尺寸相乘来设置行间距。
## 2. -webkit-text-size-adjust: 100%;
text-size-adjust 属性 允许我们控制将文本溢出算法应用到一些手机设备上。
## 3. vertical-align: baseline;
vertical-align 属性设置元素的垂直对齐方式。
## 4. audio:not([controls]) {
CSS否定伪类，:not(X)，是以一个简单的X选择器为参数的功能性标记函数。它匹配不符合参数选择器X描述的元素。X不能包含另外一个否定选择器。

:not伪类的优先级即为它参数选择器的优先级。:not伪类不像其它伪类，它不会增加选择器的优先级。

## 5.[hidden]
属性选择器

## 6. box-sizing: content-box
content-box: 元素的高度 = 内容高度 + padding + border

## 7. background-clip: padding-box;
规定背景的绘制区域
border-box	背景被裁剪到边框盒.
padding-box	背景被裁剪到内边距框.
content-box	背景被裁剪到内容框。

## 8. animation-duration: 1s;
定义动画完成一个周期需要多少秒或毫秒

## 9. animation-fill-mode: both;
动画开始前和结束后应用的css样式
none: 默认值, 不应用
forwards: 结束后用最后一帧的样式(不会回到动画播放前的状态)
backwards: 开始前用第一帧的样式(延时前)
both: 同时应用 forwards 和 backwards

## 10. text-overflow: ellipsis;
与overflow一起使用, 效果类似"aa..." (ellipsis的作用就是那三个'.'即 ...)
```css
overflow: hidden;
text-overflow: ellipsis;
```

## 11. white-space: nowrap;
## 12. vertical-align: middle;
## 13. #test[data-readmore]
选择器, id为test且含有data-readmore属性的 
## 14. transition
transition: all 0.3s ease; 
transition: max-height 0.2s ease-out;

### 语法: transition: 过渡属性 过渡效果时间 速度效果 过渡效果何时开始
#### 过渡属性(transition-property: none|all|property)
property: CSS属性名称列表, 逗号隔开
#### 过渡效果时间(transition-duration): 秒/毫秒
#### 过渡速度效果(transition-timing-function)
linear: 匀速
ease: 慢速开始 -> 加速 -> 减速 -> 慢速结束
ease-in: 慢速开始 -> 加速
ease-out: 快速开始 -> 减速 -> 慢速结束
ease-in-out: 慢速开始 -> 加速 -> 减速 -> 慢速结束
#### 过渡效果何时开始 (transition-delay: time)
transition-delay: 1s;


