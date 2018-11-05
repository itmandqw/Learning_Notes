```javascript
//初始化
$('.date-picker').datepicker({
  orientation: "bottom",
  autoclose: true,
  todayHighlight: true,
  format: 'yyyy-mm-dd',
  language: "zh-CN",
  startDate: "+3d",       //开始日期为当前日期后的三天,
})
```

```javascript
//Bug1
进行了初始化之后，startDate可能出现失效，解决办法是先销毁初始化，再重新初始化
$('.date-picker').datepicker('destroy');
then //同上初始化

//设置日期
var dt = time(new Date().getTime()+$scope.delayDay*24*3600*1000); 
$('#startDate').datepicker('setDate', dt);        //dt格式：2018-11-1

//监听日期改变
$('#startDate').on('changeDate', function(e){
  	//TODO
}
```

