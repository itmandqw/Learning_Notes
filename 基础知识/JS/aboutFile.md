# 图片上传

1,图片转base64，转格式，转blob，上传的总结<https://blog.csdn.net/wangzhanzheng/article/details/78923013>

2.压缩后上传（另附上传进度，大于一兆进行压缩处理）

<https://www.cnblogs.com/007sx/p/7583202.html>

```javascript
<html>
  <input type="file" accept="image/*" id="inputBox">
    <div>
		<img id="preview" />
	</div>
</html>
        
1,获取到上传的图片      
var input = $("#inputBox")[0];
input.onchange = function(){
  var file = input.files[0];//获取到图片
  
  var render = new FileReader();         //读取图片
  render.readAsDataURL(file);
  render.onload = function(e){
    var base64Url = e.target.result;
  	console.log(base64Url);         //base64Url即为读取到的base64格式图片
    $("#preview").attr("src",base64Url)   //预览即将上传的图片
  }
}

2.压缩图片（quality取值0-1，0.92以上默认为1）
function compressPic(path,quality){      //使用canvas.toDataURL压缩图片
  var img = new Image();
  img.src = path;       //图片路径/或图片的base64编码
  img.onload = function(){
  	var that = this;
    var w = that.width;
    var h = that.height;
    
    var canvas = document.creatElement("canvas");
    var ctx = canvas.getContext("2d");
    canvas.width = w;
    canvas.height = h;
    ctx.drawImage(that,0,0,w,h);
    
    //压缩后的base64编码。
    var compressBase64 = canvas.toDataURL("image/jpeg",quality);
    
  }
}

3,从base64图片转化为blob对象，以便上传
 function convertBase64UrlToBlob(urlData,type){
    var bytes=window.atob(urlData.split(',')[1]);//去掉url的头，并转换为byte
    
    var ab = new ArrayBuffer(bytes.length);
    var ia = new Uint8Array(ab);
    for (var i = 0; i < bytes.length; i++) {
        ia[i] = bytes.charCodeAt(i);
    }
    return new Blob( [ab] , {type : 'image/'+type});
}

4.ajax上传图片
function sendPic(){
  var file = convertBase64UrlToBlob(base64Url);//file此时为blob对象
  var formdata = new FormData();
  formdata.append("file",file1);
  $.ajax({
  	url: ""//url,
    cache: false,
    processData: false,
    contentType: false,   //使用formdata.此三者必须设置，
    success: function(data){
  		console.log(data)//todo
	}
})
}
```



# FileReader

FileReader是一种异步文件读取机制，结合input:file可以很方便的读取本地文件。file类型的input里有files属性，保存着文件的相关信息。

 ![file_detail](D:\DQW\工作学习笔记\Learning_Notes\images\file_detail.png)

可以发现input.files是一个数组，由传入的file对象组成。每个file对象包含以下属性：

lastModified：数值，表示最近一次修改时间的毫秒数；

lastModifiedDate：对象，表示最后一次表示最近一次修改时间的Date对象（高程中说是字符串，根据上图可看出应该为对象，为了层级清晰未对其展开，大家可自行查看，其可调用Date对象的有关方法，例如getDay方法）；

name：本地文件系统中的文件名；

size：文件的字节大小；

type：字符串，文件的MIME类型；

```javascript
var inputBox = document.getElementById("inputBox");
inputBox.addEventListener("change",function(){
  var reader = new FileReader();
  reader.readAsDataURL(inputBox.files[0]);   //发起异步请求
  
  reader.onload = function(e){
    //读取完成后，数据保存在对象的e.target.result属性中
    console.log(e.target.result)
    img.src = e.target.result;      //预览图片/
  }
  
})
```

 ### FileReader方法

| 方法名                | 参数         | 描述            |
| ------------------ | ---------- | ------------- |
| abort              | none       | 中断读取          |
| readAsBinaryString | file(blob) | 将文件读取为二进制码    |
| readAsDataURL      | file(blob) | 将文件读取为DataURL |
| readAsText         | file(blob) | 将文件读取为文本      |

 ![FileReader事件模型](D:\DQW\工作学习笔记\Learning_Notes\images\FileReader事件模型.png)

# img对象

img对象的事件：onload|onerror|onabort

onabort    当用户放弃图像的装载时调用的事件句柄（中断）。
onerror    在装载图像的过程中发生错误时调用的事件句柄。
onload     当图像装载完毕时调用的事件句柄。

注意：在使用src属性的时候，最好是放在onload中。

ff，chrome默认都是 window.onload 触发后，img图片描述onload才触发
而 ie 在img.onload 可能在 window.onload 还没触发 成已经触发了。



# canvas

资源链接<https://blog.csdn.net/u012468376/article/details/73350998>

如果不给canvas设置宽高，则默认width为300、height为150，单位都是px。也可以使用css属性来设置宽高，但如果宽高属性和初始比例（2：1）不一致，就会出现扭曲，所以，建议永远都不要使用css属性设备canvas的宽高。可以直接设置(下)，也可以js控制

```javascript
<canvas id="tutorial" width="300" height="300"></canvas>    ///直接设置宽高
var canvas = document.getElementById("#myCanvas");
if(canvas.getContext) return;             //如果不支持canvas
var ctx = canvas.getContext("2d");       //获得canvas上下文画布坏境
canvas.width = width;
canvas.height = height;                  //JS控制canvas宽高
```

+ canvas能绘制闭合图形，边框图形，文字等详情去看上面提供的链接。这里重点记录canvas绘制img

```javascript
  ctx.drawImage(img,x,y);    //x,y为img在canvas中的坐标
  ctx.drawImage(img,x,y,width,height)     //可用来缩放图片为自定义的宽高(width,height)
  drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)  //裁剪
   第一个参数和其它的是相同的，都是一个图像或者另一个 canvas 的引用。
  其他8个参数：
   前4个是定义图像源的裁剪位置和大小，
   后4个则是定义裁剪的目标显示位置和大小。
```

## canvas绘制基本图形小总结

+ <canvas> 只支持一种原生的 图形绘制：矩形。所有其他图形都至少需要生成一种路径(path)。不过，我们拥有众多路径生成的方法让复杂图形的绘制成为了可能。


```javascript
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
//矩形绘制
  参数说明：
 x, y：指的是矩形的左上角的坐标。(相对于canvas的坐标原点)
 width, height：指的是绘制的矩形的宽和高。
ctx.fillStyle("pink");                  //填充颜色
ctx.fillRect(x,y,width,height);         //绘制一个矩形,默认填充颜色为黑色
ctx.strokeRect(x,y,width,height)       //绘制一个矩形边框
ctx.clearRect(x,y,width,height)        //清除指定的矩形区域，然后这块区域会变的完全透明。
eg:ctx.fillRect(0,0,100,100);

//绘制路径
beginPath()   //新建一条路径
moveTo(x, y)  //把画笔移动到指定坐标(x,y).相当于设置起始点的坐标
lineTo(x,y)   //绘制一条从当前画笔位置到指定坐标(x,y)的直线
closePath()   //闭合路径
stroke()      //通过线条来绘制图形轮廓(描边)(不会自动闭合)
fill()        //通过填充路径的内容区域生成实心图形(会自动闭合)
eg: ctx.beginPath();
	ctx.moveTo(0,0);
	ctx.lineTo(100,100);
	ctx.closePath(); //到这已经绘制成一条路径，但是无法看到，需进行描边才能被看到
	ctx.stroke();


//绘制三角形
ctx.beginPath();
ctx.moveTo(30,30);
ctx.lineTo(30,130);
ctx.lineTo(130,130);
ctx.closePath();
ctx.stroke();		//绘制出一个三角形轮廓（边框）,不会自动闭合.没有closePath会绘制两条线段
ctx.fill();			//绘制出一个黑色填充的三角形，无需closePath也会自动闭合。


//绘制圆弧
1,arc(x, y, r, startAngle, endAngle, anticlockwise):
表示以(x,y)为圆心，r为半径，从startAngle弧度开始到endAngle结束画一个圆弧，anticlockwise是布尔值，true表示逆时针，false表示顺时针（默认是顺时针）
//注意: 这里的度数都是弧度。0弧度指的是x轴正方向
eg:
ctx.beginPath();
ctx.arc(50,50,40,0,Math.PI/2,false);
ctx.stroke();

2.arcTo(x1,y1,x2,y2,r):
根据给定的控制点和半径画一段圆弧，最后再以直线连接两个控制点。
eg:感觉用处不大。


//颜色样式
ctx.fillStyle = color         //设置图形的填充颜色
ctx.strokeStyle = color       //设置图形的轮廓颜色(画笔颜色)
                             //color可以是表示css颜色值的字符串，默认线条和填充颜色都是黑色。
ctx.lineWidth = value        //设置画笔线宽，默认是1.0
ctx.lineCap = type:
				//type共三个值：1，butt： 线段末端以方形结束（默认）
			   2，round: 线段末端以圆形结束
               3，square: 线段以方形结束，但增加了一个宽度与线段相同。高度是线段厚度一般的区域
eg：
		ctx.lineWidth = 20;
        ctx.lineCap = "round"
        ctx.beginPath();
        ctx.moveTo(70,40)
        ctx.lineTo(70,240);
        ctx.stroke();

ctx.lineJoin = type //同一个path内，设置线条与线条之间处的样式，取值为：round,bevel,miter
ctx.setLineDash和lineDashOffset属性来制定虚线样式：setLineDash方法接收一个数组，来指定线段与间隙的交替；lineDashOffset属性设置起始偏移量.
ctx.setLineDash([20,5])   //实现长度，虚线长度。
ctx.lineDashOffset = 10;
ctx.strokeRect(50,50,200,200);

```

## canvas绘制基本文本小总结

```javascript
ctx.fillText(text,x,y [,maxWidth]);
ctx.strokeText(text,x,y [,maxWidth]);
在指定的位置(x,y)处填充/绘制文本。绘制的最大宽度是可选的。

//绘制文本样式
ctx.font = value  //value取值(默认为10px sans-serif)和css font属性相同的字符串
textAlign = value //文本对齐选项。可选值：start(默认)，end,left,right,center
textBaseline = value //基线对齐选项。可选值：top，hanging,middle,alphabetic(默认),bottom...
direction = vallue   //文本方向.可选值：ltr,rtl,inherit(默认)
```



