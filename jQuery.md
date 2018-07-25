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
url:"http://localhost:9800/sales/hotelpayform"类似于这样的
```

