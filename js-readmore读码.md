# readmore.js 读码

## readmore.js 介绍
主要应用于有大段内容, 太占篇幅, 设置按钮折叠或展开显示

## 主要思路-个人理解
+ 先获取折叠前和折叠后两种高度并保存(用克隆的方式)
+ 根据配置以及内容高度(考虑不需要折叠的情况)设置新的高度
+ 保留当前状态(是否折叠等)



## 入口
 ```javascript
 $('#test').readmore
 ```
 
 
 ## 流程
 ### 1. 载入
 ```javascript
 $.fn.readmore = function(options) {...}
 ```
 #### 1.1 主要完成功能: 
 + 建立实例
 + 初步配置选项(字符串 or 对象)
 + $.data()装载数据. 
 
 
 重要细节: 设置selector, 即: options.selector
 ```javascript
selector = this.selector;
options.selector = selector;
```
其中 this 可以先理解为一个 jquery 对象
 
#### 1.2 跳转下一块
  ```javascript
$.data(this, 'plugin_' + readmore, new Readmore(this, options));
```

#### 1.3 全代码展示
```javascript

$.fn.readmore = function(options) {
    console.log(1);
    var args = arguments,
        selector = this.selector;

    console.log(this);

    options = options || {};

    console.log(options)
    if (typeof options === 'object') {
        return this.each(function() {
            if ($.data(this, 'plugin_' + readmore)) {
                console.log('1-1');
                var instance = $.data(this, 'plugin_' + readmore);
                instance.destroy.apply(instance);
            }

            options.selector = selector;

            $.data(this, 'plugin_' + readmore, new Readmore(this, options));
        });
    }
    else if (typeof options === 'string' && options[0] !== '_' && options !== 'init') {
        return this.each(function () {
            var instance = $.data(this, 'plugin_' + readmore);
            if (instance instanceof Readmore && typeof instance[options] === 'function') {
                instance[options].apply(instance, Array.prototype.slice.call(args, 1));
            }
        });
    }
};
```

### 2. Readmore
#### 2.1主要完成功能
+ 将用户自定义的选项与默认选项合并
+ 装载 css 样式 embedCSS (详见3)
+ 初始化readmore
+ 装载事件监听

#### 2.2 全代码展示
```javascript
function Readmore(element, options) {
    this.element = element;

    // 合并选项
    this.options = $.extend({}, defaults, options);

    embedCSS(this.options);

    this._defaults = defaults;
    this._name = readmore;

    this.init();

    // IE8 chokes on `window.addEventListener`, so need to test for support.
    if (window.addEventListener) {
        // Need to resize boxes when the page has fully loaded.
        window.addEventListener('load', resizeBoxes);
        window.addEventListener('resize', resizeBoxes);
    }
    else {
        window.attachEvent('load', resizeBoxes);
        window.attachEvent('resize', resizeBoxes);
    }
}
```


### 3. embedCSS
```css
#test + [data-readmore-toggle], #test[data-readmore]{
    display: block; width: 100%;
}

#test[data-readmore]{
    transition: height 100ms;overflow: hidden;
}
```

#### 待查问题详见附录2.1
#### 知识点 `#test[data-readmore]` 详见附录1.1
#### 完整代码
```javascript

function embedCSS(options) {
    console.log(2);
    console.log(options);
    if (! cssEmbedded[options.selector]) {
        var styles = ' ';

        // custom -s gjj
        if(!options.selector) {
            options.selector = "#test";
        }
        // custom -e gjj

        if (options.embedCSS && options.blockCSS !== '') {
            console.log('2-1');
            styles += options.selector + ' + [data-readmore-toggle], ' +
                options.selector + '[data-readmore]{' +
                options.blockCSS +
                '}';
        }

        // Include the transition CSS even if embedCSS is false
        styles += options.selector + '[data-readmore]{' +
            'transition: height ' + options.speed + 'ms;' +
            'overflow: hidden;' +
            '}';

        // console.log(styles);

        (function(d, u) {
            console.log(u);
            var css = d.createElement('style');
            css.type = 'text/css';

            if (css.styleSheet) {
                css.styleSheet.cssText = u;
            }
            else {
                css.appendChild(d.createTextNode(u));
            }


            d.getElementsByTagName('head')[0].appendChild(css);
            console.log('css');
            console.log(css);
        }(document, styles));

        cssEmbedded[options.selector] = true;
    }
}
```

### 4. init

#### 主要完成功能
+ 获取到展开前和展开后的高度
+ 获取默认展开或是折叠
+ 在文章的后添加超链接
+ 添加超链接的点击事件监听

#### 完整代码
```javascript
init: function() {
    var current = $(this.element);

    current.data({
        defaultHeight: this.options.collapsedHeight,
        heightMargin: this.options.heightMargin
    });

    setBoxHeights(current);

    var collapsedHeight = current.data('collapsedHeight'),
        heightMargin = current.data('heightMargin');

    if (current.outerHeight(true) <= collapsedHeight + heightMargin) {
        // The block is shorter than the limit, so there's no need to truncate it.
        if (this.options.blockProcessed && typeof this.options.blockProcessed === 'function') {
            this.options.blockProcessed(current, false);
        }
        return true;
    }
    else {
        var id = current.attr('id') || uniqueId(),
            useLink = this.options.startOpen ? this.options.lessLink : this.options.moreLink;

        current.attr({
            'data-readmore': '',
            'aria-expanded': this.options.startOpen,
            'id': id
        });

        current.after($(useLink)
            .on('click', (function(_this) {
                return function(event) {
                    _this.toggle(this, current[0], event);
                };
            })(this))
            .attr({
                'data-readmore-toggle': id,
                'aria-controls': id
            }));

        if (! this.options.startOpen) {
            current.css({
                height: collapsedHeight
            });
        }

        if (this.options.blockProcessed && typeof this.options.blockProcessed === 'function') {
            this.options.blockProcessed(current, true);
        }
    }
}
```

### 5. setBoxHeights
#### 主要完成功能
+ 将原来的 jquery 对象克隆, 并将高度设为自动(即不折叠)
+ 将克隆后的对象追加到原对象后
+ 删除原对象
+ 保留折叠后的高度, 折叠前的高度, 最大高度
+ 将 css 样式中设置的'最大高度'清空

#### 复杂代码
```javascript
cssMaxHeight = parseInt( el.css({maxHeight: ''}).css('max-height').replace(/[^-\d\.]/g, ''), 10),
```
运行了一下返回的结果是NaN, 进一步运行发现`el.css({maxHeight: ''}).css('max-height')` 的结果是none
将`el.css({maxHeight: ''})` 换成 `el.css({maxHeight: '50px'})` 运行结果是50

#### 完整代码
```javascript
function setBoxHeights(element) {
    var el = element.clone().css({
            height: 'auto',
            width: element.width(),
            maxHeight: 'none',
            overflow: 'hidden'
        }).insertAfter(element),
        expandedHeight = el.outerHeight(),
        cssMaxHeight = parseInt(el.css({maxHeight: ''}).css('max-height').replace(/[^-\d\.]/g, ''), 10),
        defaultHeight = element.data('defaultHeight');

    el.remove();

    var collapsedHeight = cssMaxHeight || element.data('collapsedHeight') || defaultHeight;

    // Store our measurements.
    element.data({
        expandedHeight: expandedHeight,
        maxHeight: cssMaxHeight,
        collapsedHeight: collapsedHeight
    })
    // and disable any `max-height` property set in CSS
        .css({
            maxHeight: 'none'
        });
}
```

### 6. toggle
#### 调用用例
```javascript
_this.toggle(this, current[0], event);
```
#### 完整代码
```javascript
toggle: function(trigger, element, event) {
    if (event) {
        event.preventDefault();
    }

    if (! trigger) {
        trigger = $('[aria-controls="' + this.element.id + '"]')[0];
    }

    if (! element) {
        element = this.element;
    }

    var $element = $(element),
        newHeight = '',
        newLink = '',
        expanded = false,
        collapsedHeight = $element.data('collapsedHeight');

    if ($element.height() <= collapsedHeight) {
        newHeight = $element.data('expandedHeight') + 'px';
        newLink = 'lessLink';
        expanded = true;
    }
    else {
        newHeight = collapsedHeight;
        newLink = 'moreLink';
    }

    // Fire beforeToggle callback
    // Since we determined the new "expanded" state above we're now out of sync
    // with our true current state, so we need to flip the value of `expanded`
    if (this.options.beforeToggle && typeof this.options.beforeToggle === 'function') {
        this.options.beforeToggle(trigger, $element, ! expanded);
    }

    $element.css({'height': newHeight});

    // Fire afterToggle callback
    $element.on('transitionend', (function(_this) {
        return function() {
            if (_this.options.afterToggle && typeof _this.options.afterToggle === 'function') {
                _this.options.afterToggle(trigger, $element, expanded);
            }

            $(this).attr({
                'aria-expanded': expanded
            }).off('transitionend');
        }
    })(this));

    $(trigger).replaceWith($(this.options[newLink])
        .on('click', (function(_this) {
            return function(event) {
                _this.toggle(this, element, event);
            };
        })(this))
        .attr({
            'data-readmore-toggle': $element.attr('id'),
            'aria-controls': $element.attr('id')
        }));
}
```



## 附录
### 1. 知识点
#### 1.1  #test[data-readmore]
选择器, id为test且含有data-readmore属性的 

### 2. 待查
#### 2.1 dom.stylesheet
用例: 

```javascript
var css = d.createElement('style');
if (css.styleSheet) {
    css.styleSheet.cssText = u;
}
```

### 3. 代码段收集