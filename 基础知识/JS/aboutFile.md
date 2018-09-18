# FileReader

FileReader是一种异步文件读取机制，结合input:file可以很方便的读取本地文件。file类型的input里有files属性，保存着文件的相关信息。

 ![file_detail](images\file_detail.png)

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

![FileReader事件模型](images\FileReader事件模型.png)

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



  ​



