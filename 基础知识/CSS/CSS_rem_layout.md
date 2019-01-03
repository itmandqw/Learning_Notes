## 1.JS动态计算html的font-size方法

例子：<https://mtrip.csair.com/waptuan/action/CouponAction.shtml?mhd=goodDetail&goodsid=D1812280000003?WT.mc_id=FX1812290001-ybb>

```javascript

(function (doc, win) {
          var docEl = doc.documentElement,
            resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
            recalc = function () {
              var clientWidth = docEl.clientWidth;
              if (!clientWidth) return;
			  if(clientWidth<378){
				   docEl.style.fontSize = 20 * (clientWidth / 320) + 'px';
				  }
			   else 	  
             docEl.style.fontSize = 23.4375  + 'px';
            };

          if (!doc.addEventListener) return;
          win.addEventListener(resizeEvt, recalc, false);
          doc.addEventListener('DOMContentLoaded', recalc, false);
        })(document, window);
```

