# 通过`userAgent`属性判断浏览器

navigator是JavaScript中的一个独立的对象，用于提供用户所使用的浏览器以及操作系统等信息，以navigator对象属性的形式来提供。所有浏览器都支持该对象。

navigator对象有一个userAgent属性，会返回用户的设备操作系统和浏览器的信息。

```javascript
谷歌浏览器中F12 console控制台输入navigator.userAgent
返回"Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/57.0.2987.98 Safari/537.36"详情如下：

1）Mozilla/5.0:表示兼容Mozilla，几乎所有的浏览器都有这个字符
2) (Windows NT 6.1; Win64; x64): 表示设备的操作系统版本，以及CPU信息；
3）AppleWebKit/537.36 (KHTML, like Gecko)：表示浏览器的内核；
4) Chrome/57.0.2987.98 Safari/537.36: 表示浏览器的版本号。

手机或平板设备中返回的字符串都会包含`Mobile`
```

## 根据navigator.userAgent返回值识别 浏览器

```javascript
function validBrowser(){ 
  var u_agent = navigator.userAgent; 
  var browser_name='Failed to identify the browser'; 
  if(u_agent.indexOf('Firefox')>-1){ 
  browser_name='Firefox'; 
  }else if(u_agent.indexOf('Chrome')>-1){ 
  browser_name='Chrome'; 
  }else if(u_agent.indexOf('Trident')>-1&&u_agent.indexOf('rv:11')>-1){ 
  browser_name='IE11'; 
  }else if(u_agent.indexOf('MSIE')>-1&&u_agent.indexOf('Trident')>-1){ 
  browser_name='IE(8-10)'; 
  }else if(u_agent.indexOf('MSIE')>-1){ 
  browser_name='IE(6-7)'; 
  }else if(u_agent.indexOf('Opera')>-1){ 
  browser_name='Opera'; 
  }else{ 
  browser_name+=',info:'+u_agent; 
  }
  return browser_name;
}
```

## 区分安卓和ios

```javascript
var u = navigator.userAgent;
var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1; //android终端
var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
alert('是否是Android：'+isAndroid);
alert('是否是iOS：'+isiOS);
```

