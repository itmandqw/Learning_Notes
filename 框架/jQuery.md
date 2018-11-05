# $.ajax

配置全局loading：

```javascript
#loading是一个loading的图片，默认display:none;
$(document).ajaxSend(function(evt,request,settings){
    	$("#loading").css("display","block");
});
$(document).ajaxComplete(function(evt,request,settings){
    	$("#loading").css("display","none");
});
$(document).ajaxError(function(evt,request,settings){
    	$("#loading").css("display","none");
});
//这样配置完成后，每次发送ajax请求都会出现loading,请求完毕loading消失。
```

## $.ajax(post)

```javascript
contentType:"application/json"
data:JSON.stringify(参数对象)
//这两个配合使用传给后端的是正经的json对象
{
  name: "dqw",
  age: 18
}
类似于这样的
```



#     `forEach` &  `$("selector").each()` & `$.each()`

区别见<https://www.cnblogs.com/longailong/p/6409172.html>

1.`forEach`是原生JS中遍历数组的方法

```javascript
对数组中的每一项执行一个函数，没有返回值(undefined)
var arr = [1,2,3,4];
arr.forEach(function(item,index){
  console.log("索引为"+ index + "的值为" + item);
})
```

2,`$.each()`是jquery中遍历数组的方法

```javascript
就是一个对数组中的每个元素进行一次函数处理。
var arr = [1,2,3,4];
$.each(arr,function(index,item){
  console.log("索引为"+ index + "的值为" + item);
})
```

3,`$("selector").each()`规定为每个匹配到的元素运行相同的函数。

```javascript
$("button").click(function(){
  $("li").each(function(){
  		console.log($(this).text())   //点击按钮输出每个li的文本值。
        console.log($(this))          //打印出jQ封装的li的Dom对象
        console.log(this)             //打印出原生Dom标签包括标签内容
 	})
})
$("li")中是一个jQ封装的li元素集合。但不是一个数组，所以也不能使用数组里的方法。可通过本方法将其转换成一个真正的数组。如上
```

# jQ判断点击区域是否为指定区域

```javascript
//catalog_modal最外层区域
//最外层区域内部有一些小区域，当点击某些小区域时候起作用
$(".catalog_modal").click(function(e){
    e = window.event || e; // 兼容IE7
    obj = $(e.srcElement || e.target);
    if ($(obj).is(".catalogs, .catalogs a, .title")) {
        //TODO 在小区域.catalogs, .catalogs a, .title中有效
    } else {
        //除上述区域外的范围有效
    }
});
```

## jQ事件绑定on

```javascript
$.on作用与delegte,live,bind类似， jQ 1.9已经正式废弃live
正常情况下，我们都是这样绑定一个点击事件对吧，
$("selector").click(function(){
	//TODO
})
但如果这个selector是js动态生成的dom呢，上面的方法就无能为力了，导致绑定不上事件，解决方法如下：
$("existSelector").on('click','selector',function(){
  //TODO
})
//existSelector必须是页面中已经存在的CSS选择器。
```

