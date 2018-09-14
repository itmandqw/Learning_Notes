# 媒体查询

语法：@media screen and (min-width: 400px){ ... }

**1. 最大宽度max-width**

“max-width”是媒体特性中最常用的一个特性，其意思是指媒体类型小于或等于指定的宽度时，样式生效。

```css
@media screen and (max-width:480px){
 .ads {
   display:none;
  }
}
上面表示的是：当屏幕小于或等于480px时,页面中的广告区块（.ads）都将被隐藏。
```

**2.最小宽度min-width**"

min-width"与之相反，意思是当媒体类型大于或等于指定宽度的时候，样式生效。

```CSS
@media screen and (min-width:900px){
.wrapper{width: 980px;}
}
上面表示的是：当屏幕大于或等于900px时，容器“.wrapper”的宽度为980px。
```

**3.多个媒体特性联合使用**

可以使用关键词**"and**"将多个媒体特性结合在一起，举例如下：

```css
当屏幕在600px~900px之间时，body的背景色渲染为“#f5f5f5”，如下所示。
@media screen and (min-width:600px) and (max-width:900px){
  body {background-color:#f5f5f5;}
}
```

**4.常用的方法**

```css
@media screen and (max-width:1200px)

@media screen and (max-width:992px)

@media screen and (max-width:768px)

@media screen and (max-width:480px)
  
@media screen and (max-width:320px)

在设置时，需要注意先后顺序，不然后面的会覆盖前面的样式。
  
@media screen and (min-width:320px)

@media screen and (min-width:480px)

@media screen and (min-width:768px)

@media screen and (min-width:992px)
  
@media screen and (min-width:1200px)
  
and 后边有个空格千万不能忘了。不然会导致样式失效。
```

**5.CSS判断手机横竖屏**

```CSS
@media screen and (orientation: portrait) {
    竖屏 css
}
@media screen and (orientation: landscape) {
    横屏 css
}
```

**5.1扩充（JS监听手机横竖屏）**

```javascript
window.addEventListener("onorientationchange" in window ? "orientationchange" : "resize", function() {
    if (window.orientation === 180 || window.orientation === 0) {
        alert('竖屏状态！');
}
    if (window.orientation === 90 || window.orientation === -90 ){
        alert('横屏状态！');
    } 
}, false);
//移动端的浏览器一般都支持window.orientation这个参数，通过这个参数可以判断出手机是处在横屏还是竖屏状态
```

# text

+ white-space: nowrap(禁止文字换行)

+ text-overflow: ellipsis(当对象内文本溢出时显示省略标记...)

+ img标签指定-webkit-touch-callout:none也会禁止设备弹出列表按钮，这样用户就无法保存/复制你的图片了。

+ 文字标签的-webkit-user-select:none(禁止ios用户选中文字);

+ text-overflow: ellipsis(当对象内文本溢出时显示省略标记...)

***

# CSS选择器

