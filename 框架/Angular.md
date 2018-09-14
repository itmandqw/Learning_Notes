# Angular

## 1,input

checkbox  的ng-model的值总是true或false,而其他的ng-model的值默认都是value的值。

在任何时候，单选框或多选框一定要加上label属性，提升用户体验，通常label可以用for属性去对应input的id.更简单的方法可以直接用label标签包裹住对应的input从而省去for属性和没有必要的id值。

当input的type = file时候，ng-change事件是无效的，可以用原生onchange事件代替。

onchange事件在内容改变（两次内容有可能相等）且失去焦点时触发；

onpropertychange事件是实时触发，每增加或删除一个字符就会触发，通过js改变也会触发该事件，但是该事件是IE专有。

oninput事件是IE之外的大多数浏览器支持的事件，在value改变时实时触发，但是通过js改变value时不会触发；onpropertychange事件是任何属性改变都会触发，而oninput却只在value改变时触发，oninput要通过addEventListener()来注册，onpropertychange注册方法与一般事件相同。

```javascript

$("#keyword").bind("input propertychange",function(){
	console.log($("#keyword").val())
});
```





