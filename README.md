# FuckWebBUG
**看名字，懂意思**

本文会将HTML，CSS，JavaScript中遇到的问题整理出来，每个标签、样式、API都会整理到一起，使用的时候搜索标签、样式、API就能找到相关的BUG问题

### 布局

1.

在拿到设计稿的时候先要将页面上的元素拆分，在遇到动态加载的数据标签时应当思考容器里所放入的数据是否会影响布局，做到不让数据影响布局，布局能不被数据的长度宽度打乱，如果页面上没给出数据的极限值就由我们自己去思考如何制造极限值然后去解决

```html
<p>99999999999999999999999999999999999</p>

```

```css
p{
 width:2.33px;
}
```

2.

元素之间不要相互影响，如果p标签的数据过长会导致span标签被挤出，所以不应该用margin-left来扩展两个元素之间的宽度，应该让span跟随其变化，不要使用固定值

```html
<p>22222</p><span>3333</span>
```
```css
span{
 margin-left:233px;
}


```



### HTML

1.

> input

> type='range'

input标签的type属性为range的时候，对小数点会自动变成取整

### CSS

1.

> display:table-cell

> vertical-align:middle

display: table-cell;与float: left;存在冲突，float会导致display无法实现

2.

> display:table-cell

> vertical-align:middle

针对图片垂直居中问题，图片是个置换元素，有些特殊的特性。

```css
.box {
 display: table-cell;
 vertical-align:middle;

 text-align:center;

 *display: block;
 *font-size: 175px;
 *font-family:Arial;

 width:200px;
 height:200px;
 border: 1px solid #eee;
}
.box img {
 vertical-align:middle;
}
```

3.

> display:table-cell

> vertical-align:middle

在遇到位置出问题的情况下，仔细看好上下文所设置的元素，特别是在图片的居中问题上，应该多往父元素去看css，因为很多时候可能是父元素的css问题。

4.

> background 

[base-48转换](http://c.runoob.com/front-end/59)

有时候很多“雪碧图”可以将其转换成base-48来直接放入css样式里，这样就可以节省我们在background里去找文件引用

### JavaScript

1.将功能封装，以便以后重复使用

### Jquery

1.

> $()

$(fuck.a,fuck.b)是找不到的，Jquery中的语法是在第二个中找第一个，正确的写法是$(fuck.a+','fuck.b)用逗号分隔开

### 插件

1.

使用单步调试查看别人的代码是怎么运行的，针对自己写插件的时候扩展思路很有必要

> step into：进入子函数

> step over：越过子函数，但子函数会执行

> step out：跳出子函数

step into就是单步执行，遇到子函数就进入并且继续单步执行；

step over是在单步执行时，在函数内遇到子函数时不会进入子函数内单步执行，而是将子函数整个执行完再停止，也就是把子函数整个作为一步。有一点,经过我们简单的调试,在不存在子函数的情况下是和step into效果一样的。

step out就是但单步执行到子函数内时，用step out就可以执行完子函数余下部分，并返回到上一层函
数。

2.

> jQuery插件

> 插件初始化

> 插件基本代码

```JavaScript
(function($) {
  var pluginName = 'slide';//定义插件名字
    var defaults = {//定义插件基础配置
        slideBox:'slide-box'
    }

  function Plugin(element, options) {//插件初始化设置
    this.options = $.extend({}, defaults, options); //后面覆盖前面

    this.init();//开始初始化
  }

  Plugin.prototype.init = function() {//将初始化封装，功能都可通过此方法添加
    console.log(this.options.slideBox);
    this.onClick();
  };

  $.fn[pluginName] = function(options) {//插件对外接口
    var args = Array.prototype.slice.call(arguments, 1);//获取字符串之后的参数
    
    //Array 表示JavaScript中的Array类
    //prototype 表示调用Array的方法
    //slice 表示
    //call 表示

    return this.each(function() {//回调，整个插件的开始
      var $this = $(this),
        data = $this.data('plugin_' + pluginName);

      // Create a new instance.
      if (!data) {
        $this.data('plugin_' + pluginName, (data = new Plugin(this, options)));//关键点在new 开始实例化所传入的参数
      }

      // Make it possible to access methods from public.
      // e.g `$element.rangeslider('method');`
      if (typeof options === 'string') {
        data[options].apply(data, args);
      }
    });
  };

})(jQuery);

```
