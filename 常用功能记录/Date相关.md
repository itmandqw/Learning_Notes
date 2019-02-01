## 常用获取时间的方法

```javascript

var myDate = new Date();    //Wed Sep 19 2018 09:20:30 GMT+0800 (中国标准时间)

myDate.getTime(); //获取当前时间(从1970.1.1开始的毫秒数) 时间戳！！
 
myDate.getFullYear(); //获取完整的年份(4位,1970-????)
 
myDate.getMonth(); //获取当前月份(0-11,0代表1月)通常计算出来的月份+1获得当前月份
 
myDate.getDate(); //获取当前日(1-31)
 
myDate.getDay(); //获取当前星期X(0-6,0代表星期天)
 
myDate.getHours(); //获取当前小时数(0-23)
 
myDate.getMinutes(); //获取当前分钟数(0-59)
 
myDate.getSeconds(); //获取当前秒数(0-59)
//以上获取的get可以改成set即变成了设置时间，设置完的时间默认返回的是时间戳，看需求转换

//以下不太常用~ 
myDate.getMilliseconds(); //获取当前毫秒数(0-999)

myDate.toLocaleString(); //获取日期与时间             2018/9/19 上午9:04:03
 
myDate.toLocaleDateString(); //获取当前日期            2018/9/19
  
myDate.toLocaleTimeString(); //获取当前时间            上午9:04:03

console.log(myDate)           //Wed Sep 19 2018 09:20:30 GMT+0800 (中国标准时间)

myDate.toTimeString()  //获取时间          09:04:03 GMT+0800 (中国标准时间)

myDate.toDateString()   //获取日期                    Wed Sep 19 2018
```

## 时间戳转换为YY-mm-dd hh:mm:ss格式

```javascript
//将时间戳转换为时间YY-mm-dd hh:mm:ss
function timeFormat(time){
	function add(m){
		return m< 10? m="0"+m:m
	}
  	//参数time注意：时间戳为10位需*1000，时间戳为13位的话不需乘1000
	var dt = new Date(time);
	var year = dt.getFullYear();
	var month = dt.getMonth() + 1;
	var day = dt.getDate();
	var hh = dt.getHours();
	var mm = dt.getMinutes();
	var ss = dt.getSeconds();
	
	return year + "-" + add(month) + "-"+ add(day) + "  " + add(hh)+ ":" + add(mm)+ ":" + add(ss);
}
```

## 时间戳转化为yyyy-mm-dd格式：

```javascript
//将时间戳转换为时间YY-mm-dd
function timeTrans(time){
	function add(m){
		return m<=0? m="0"+m:m
	}
	var dt = new Date(time);

	var year = dt.getFullYear();
	var month = dt.getMonth() + 1;
	var day = dt.getDate();
	
	return year + "-" + add(month) + "-"+ add(day);
}
```

## 日期格式转换成时间戳

```javascript
var date = new Date('2014-04-23 18:55:49:123');
    // 有三种方式获取
    var time1 = date.getTime();
    var time2 = date.valueOf();
    var time3 = Date.parse(date);     //parse是Date的方法。语法与上述两种也不同。
    console.log(time1);//1398250549123
    console.log(time2);//1398250549123
    console.log(time3);//1398250549000
    //以上三种获取方式的区别：
　　//第一、第二种：会精确到毫秒
　　//第三种：只能精确到秒，毫秒用000替代

//还有两种方法：也是精确到毫秒
//将时间转换成一个number类型的数值，即时间戳
var time4 = Number(date);
console.log(time4);   //1537321261284
//ES5给Date提供了一种获取时间戳的新特性：
var time5 = Date.now();
console.log(time5);      //1537321482177
```

细节注意：

```javascript
<input type = "date" id = "end" value = "2018-09-04" />
//若用value去初始化input的值，格式必须为   "YYYY-MM-DD" 否则报错。yyyy-m-d也不行,必须补全
显示在页面中格式为"YYYY/MM/DD"
input type = "type" 中max 和 min也同理。mm-dd必须为两位数。不足两位加0不足。否则无效。
```

