# 简单的浏览器唯一性确定

JavaScript判断浏览器类型一般有两种办法，一种是根据各种浏览器独有的属性来分辨，另一种是通过分析浏览器的userAgent属性来判断的。在许多情况下，值判断出浏览器类型之后，还需判断浏览器版本才能处理兼容性问题，而判断浏览器的版本一般只能通过分析浏览器的userAgent才能知道。

## IE

**只有IE浏览器支持ActiveXObject函数。所以可通过window.ActiveXObject来判断是否为IE浏览器。**

## Filefox

**Firefox中的DOM元素都有一个getBoxObjectFor函数，用来获取该DOM元素的位置和大小，这是Firefox独有的，判断他即可知道当前浏览器是Firefox**(目前已不建议使用)

## Opera

**Opera提供了专门的浏览器标志，就是window.opera属性。**

## Safari

**Safari浏览器中有一个其他浏览器没有的openDatabase函数，可做为判断Safari的标志。**

## Chrome

**Chrome有一个MessageEvent函数，但Firefox也有。不过，好在Chrome并没有Firefox的getBoxObjectFor函数，根据这个条件还是可以准确判断出Chrome浏览器的。**

~~~javascript
//简单的判断浏览器（旧）
if(window.ActiveXObject){
  console.log("IE")      //IE浏览器下的代码
}else if(window.opera){
  console.log("Opear")   //Opera
}else if(window.openDatabase){
  console.log("Safari")     //Safari
}else if(document.getBoxObjectFor){
  console.log("Firefox")    //目前已不建议使用
}else if(window.MessageEvent && !document.getBoxObjectFor){
  console.log("chrome")
}

//（新）
var ua = navigator.userAgent.toLowerCase();
if(ua.indexOf('firefox')>-1 ){
    //是Firefix
}
else if (window.MessageEvent && !document.getBoxObjectFor && ua.indexOf('chrome') > -1) {
  //是chrome
}
~~~

***

# CMD、AMD、CommonJS 规范

### 目的：

首先一个模块是为了实现特定功能的文件，模块化就是使用模块来隔离、组织复杂的javascript代码（代码量过万）。模块化的目的是为了解决两大难题，一是命名冲突，二是依赖管理。当然它还有其他长处，可以提高代码的可读性，提高复用性。

### CommonJs

 服务器端的Node.js遵循CommonJS规范。核心思想是允许模块通过require 方法来同步加载所要依赖的其他模块，然后通过 exports或module.exports来导出需要暴露的接口。(如node.js)

- 同步的模块方式不适合不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的
- 不能非阻塞的并行加载多个模块

CommonJS是同步加载模块

1.定义模块：一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，也就是说，在该模块内部定义的变量，无法被其他模块读取，除非定义为global（全局）对象的属性
2.模块输出： 模块只有一个出口，module.exports对象，我们需要把模块希望输出的内容放入该对象
3.加载模块： 加载模块使用require方法，该方法读取一个文件并执行，返回文件内部的module.exports对象

## AMD

AMD 即Asynchronous Module Definition，中文名是**异步模块定义**的意思。它是一个在浏览器端模块化开发的规范，模块和依赖可以异步加载，对浏览器端较为适用。可以说AMD是专门为浏览器中的javascript环境设计的规范。语法如下：
define(id?, dependencies?, factory)
1.id: 定义中模块的名字，可选；如果没有提供该参数，模块的名字应该默认为模块加载器请求的**指定脚本的名字(如dialog.js)**
2.依赖dependencies：是一个当前模块依赖的，已被模块定义的模块标识的数组字面量。 依赖参数是可选的，如果忽略此参数，它应该默认为["require", "exports", "module"]。然而，如果工厂方法的长度属性小于3，加载器会选择以函数的长度属性指定的参数个数调用工厂方法。
3.工厂方法factory，模块初始化要执行的函数或对象。如果为函数，它应该只被执行一次。如果是对象，此对象应该为模块的输出值

### CMD

CMD 即Common Module Definition通用模块定义，CMD规范是国内发展出来的，就像AMD有个requireJS，CMD有个浏览器的实现SeaJS，SeaJS要解决的问题和requireJS一样，只不过在模块定义方式和模块加载（可以说运行、解析）时机上有所不同。语法如下：
define(id?, deps?, factory)

### AMD和CMD区别：

AMD与CMD的区别
1.对于依赖的模块，AMD 是**提前执行**，CMD 是**延迟执行**。
2.CMD 推崇**依赖就近**，AMD 推崇**依赖前置**。
3.AMD 的 API 默认是**一个当多个用**，CMD 的 API 严格区分，推崇**职责单一**。比如 AMD 里，require 分全局 require 和局部 require，都叫 require。CMD 里，没有全局 require，而是根据模块系统的完备性，提供 seajs.use 来实现模块系统的加载启动。CMD 里，每个 API 都**简单纯粹**

简单来说。AMD所依赖的模块是刚开始就全部引入，如果在写的过程中发现需要新的模块，必须找到开始加载模块的地方加进去所需要的模块，而CMD是随时用到随时添加。

# 页面清除缓存方法

```html
<!--关闭所有缓存选项-->
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=8">
    <meta http-equiv="Expires" content="0">
    <meta http-equiv="Pragma" content="no-cache">
    <meta http-equiv="Cache-control" content="no-cache">
    <meta http-equiv="Cache" content="no-cache">
```

