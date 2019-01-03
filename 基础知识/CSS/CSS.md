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

+ 属性选择器

  ```	javascript
  input[type="text"]{width:100px}
  //为type为text文本框的input设置宽度
  ```

+ 部分属性选择器（CSS2新增）

  ```javascript
  p[class~="name"]{color:red}
  //匹配到类名存在name的元素颜色设置红色 eg： <p class = "name  text">
  ```

+ 子串匹配属性（CSS3新增）

  ```css
  [abc^="def"]    //选择 abc 属性值以 "def" 开头的所有元素
  [abc$="def"]    //选择 abc 属性值以 "def" 结尾的所有元素
  [abc*="def"]    //选择 abc 属性值中包含子串 "def" 的所有元素
  ```

+ 其中[abc*="def"]和[abc~="def"]的区别:

  ```css
  <p class="class1  class2"></p>
  p[class~="class1"]  //选择成功
  p[class~="class1 clas"]  //选择失败
  p[class*="class1 clas"]  //选择成功 
  //而且，无论哪种方式，css样式顺序必须严格对照p的类名里面的顺序，包括空格都要一致，比如p[class*="clas class1"]就选择不到，当然，多个空格少个空格也是选择不到
  ~针对的是 “单一类名”的字符串，而*比较强大，针对的是所有类组合看成一个字符串
  ```

  ## 子元素选择器  >  nth-child(n),nth-of-type(n)区别

  ```css
  //      > 比较常见，用于选择当前标签的所有子元素,此处略过
  <div class="container">
      <p>~~~~~~~</p>          //(1)
      <li>~~~~~~~~</li>       //(2)
      <li>~~~~~~~~</li>       //(3)
      <p>~~~~~~~~</p>         //(4)
      <p>~~~~~~~~</p>			//(5)
      <li>~~~~~~~~</li>		//(6)
     <li>~~~~~~~~</li>		//(7)
  </div>

  nth-child(n): 位置优先，用于选择当前标签下的第n个元素，若选中位置没有该标签，则选择失败。(即如果第n个位置不是这个标签算选择失败)
  li:nth-child(2){
    color: red;                  //(2)文字渲染为红色
  }
  //中间加一个空格，代表没有指定标签
  .container  :nth-child(2){color: red;}        //(2)被文字变为红色
  //可以理解为选择container下的第二个子元素

  nth-of-type(n)：标签优先，用于选择父元素下的第n个指定标签子元素(先看父元素下有这个标签没有，有的话看这个标签第n个)

  li:nth-of-type(2){
    color: red;                  //(3)文字渲染为红色
  }  //代表在li的父元素下寻找第二个li 

  //中间加一个空格，代表没有指定元素
  .container  :nth-of-type(2){}  
  //选择container父元素下每种标签的第二个子元素，即寻找所有标签的第二个
  ```

  ## 兄弟选择器： + 和 ~

  ```css
  li:nth-child(2) + li{}   //选择第二个元素之后的一个" 同级 "元素      //(3)

  li:nth-child(2) ~ p{}   //选择第二个元素之后所有的" 同级 "元素     (4)(5)
  ```

  ​

  # 去掉默认滚动条样式

  ```css
  如果需去掉默认样式只需第一步宽高都为零即可
  .gundong_wrap::-webkit-scrollbar {/*滚动条整体样式*/
      width: 4px;     /*高宽分别对应横竖滚动条的尺寸*/
      height: 4px;
  }
  .gundong_wrap::-webkit-scrollbar-thumb {/*滚动条里面小方块*/
      border-radius: 5px;
      -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
      background: rgba(0,0,0,0.2);
  }
  .gundong_wrap::-webkit-scrollbar-track {/*滚动条里面轨道*/
      -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
      border-radius: 0;
      background: rgba(0,0,0,0.1);
  }
  ```

  ​

