# svg 学习
## `<svg>` 开始标签
`width`, `height` svg图片的宽高

## circle 圆
cy, cx 圆心的坐标, 默认(0, 0)  
r 半径
+ stroke: 外边框颜色  
+ stroke-width: 外边框宽度  
+ fill: 圆填充的颜色  

## rect 矩形
- x: 距左边的距离
- y: 距顶部的距离
- rx, ry: 矩形圆角
### style: css样式
+ fill 填充  
+ stroke-width: 边框宽度  
+ stroke: 边框颜色  
+ fill-opacity: 填充色的透明度
+ 边框的透明度
+ opacity: 整个元素的透明度
## ellipse 椭圆
+ cx, cy: 圆点坐标
+ rx, ry 水平半径, 垂直半径
## line 线
## polyline 折线
## polygon 多边形
## path 路径

## viewBox 与 viewPort
viewPort指的就是
```html
<svg width="" height="300"></svg>
```
将viewBox里的区域全屏居中显示
```html
<svg width="400" height="300" viewBox="0,0,40,30" style="border:1px solid #cd0000;">
    <rect x="10" y="5" width="20" height="15" fill="#cd0000"/>
</svg>
```


## preserveAspectRation
适用情况: viewPort 与 viewBox 不比例不那么匹配的时候    
```html
preserveAspectRation="xMinYMid slice"
```
第1个值表示，viewBox如何与SVG viewport对齐；第2个值表示，如何维持高宽比（如果有）   
第1个值又是由两部分组成的。前半部分表示x方向对齐，后半部分表示y方向对齐   
值	    含义
xMin	viewport和viewBox左边对齐   
xMid	viewport和viewBox x轴中心对齐   
xMax	viewport和viewBox右边对齐   
YMin	viewport和viewBox上边缘对齐。注意Y是大写。   
YMid	viewport和viewBox y轴中心点对齐。注意Y是大写。   
YMax	viewport和viewBox下边缘对齐。注意Y是大写。   


第2个值
值	含义
meet	保持纵横比缩放viewBox适应viewport，受
slice	保持纵横比同时比例小的方向放大填满viewport，攻
none	扭曲纵横比以充分适应viewport，变态

## transform="matrix(1,0,0,1,51.125,36.0977)"
矩阵
[a, c, e]
[b, d, f]
[0, 0, 1]
得出 x = ax + cy + e, y = bx + dy + f
所以matrix(1, 0, 0, 1, 51.125, 36.0977) 代入得
x = x + 51.125
y = y + 36.0977
效果就是: 向右移动了51.125, 向下移动了36.0977

## 文本换行写法
```html
<text x="35" y="50" style="fill: #000;">使用说明书
    <tspan x="50" y="70">合格证</tspan>
</text>
```

## maker
## DOM操作
1. SVG DOM 在 Document Object Model HTML 之后构建

##其他笔记
```javascript
(function(){})();
(var a = function(){})();
```