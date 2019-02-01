1,手机号+短信验证码+倒计时

```html
html结构大致如下：
<ul class="form-group">
  <li><span>手机号</span><input type="tel" id="tel" /></li>
  <li><span>图形码</span><input type="text" id="kaptcha" />
    <img id="kaptcha-img" />
  </li>
  <li>
    <span>验证码</span><input type="text" id="vcode" />
    <a class="btn btn-info send" id="sendcd">获取验证码</a>
  </li>
</ul>
```



```javascript
	//基本Js逻辑	
	var nt=0;
		$("#sendcd").click(function(){
			console.log("getCode");
			var tel = $("#tel").val();
			var kaptcha=$("#kaptcha").val();
			console.log(tel,kaptcha);
			var url = '/rest/user/send';
			var postStr = 'tel=' + tel+'&kaptcha='+kaptcha;
			var v = -1;
			if(tel==""){
				popMsg("请先填写手机号",'info');
			}else{
				if(kaptcha==""){
					popMsg("请输入图形验证码",'info');
				}else{
					if(nt==0){
						nt=1;
						$(".send").css({
							"background-color":"#6a787b",
							"border-color":"#6a787b"
						})
				   		$.ajax({
				   			url: url,
				   			method: "POST",
				   			data:postStr,
				   			success: function(data){
				   				$(".send").css({
									"background-color":"#5bc0de",
									"border-color":"#46b8da"
								})
				   				v = data;
				   				if (v == 0) {
					   				popMsg('短信发送成功', 'success');
					   				fun();
					   			} else if(v == 1) {
					   				popMsg("发送失败，请检查手机号", 'info');
					   				nt = 0;
					   			}else{
					   				popMsg("图形验证码填写错误", 'info');
					   				nt = 0;
					   			}
				   			},
				   			error:function(){
				   				$(".send").css({
									"background-color":"#5bc0de",
									"border-color":"#46b8da"
								});
				   				popMsg("系统异常","danger");
					   			nt = 0;
				   			}
				   		})
					}
			   	
				}
			}
			
		}) 
		
		$("#kaptcha-img").click(function(){
			kaptchaC();
		})
		
		function kaptchaC(){
/* 			/kaptcha.jpg?1533692211000
			var img=$("#kaptcha-img").attr("src"); */
			var mn = "/kaptcha.jpg";
			var timestamp = Date.parse(new Date());
			$("#kaptcha-img").attr("src",mn+"?"+timestamp);	
		}
	
		var timer = null;
		var i = 60;
		function fun() {
			$("#sendcd").attr( "disabled", "disabled");
			$("#sendcd").text(i + "秒后重发");
			timer = setInterval("show()", 1000);//设置定时器,1000ms调用一次show()
		}

		function show() {
			i--;
			if (i == 0) {
				kaptchaC();
				$("#sendcd").attr( "disabled", null);
				$("#sendcd").text("获取验证码");
				i = 60;
				clearInterval(timer);//清除定时器
				nt = 0;
			} else {
				$("#sendcd").text(i + "秒后重发");
			}
		}
		
		
	 var v='';
	 var st=0;
    $("#login").click(function(){
    	var tel = $("#tel").val();
    	var name = "";
    	var vcode = $("#vcode").val();
    	var url = '/rest/user/verify';
    	var postStr = 'tel=' + tel + "&name=" + name + "&vcode=" + vcode;
    	if(st==0){
			st=1;
	   		$.ajax({
	   			url:url,
	   			method: "POST",
	   			data: postStr,
	   			success:function(data){
	   				v = data;
	   				if (v == 0) {
		    			login();
		    		} else {
		    			popMsg('验证码输入错误或已失效，请重新输入', 'danger');
		    			st = 0;
		    		}
	   			},
	   			error:function(){
		   			popMsg("系统异常","danger");
		   			st = 0;
		   		}
	   		})
    	}
    })
    
    var loginSucc=false;
    function login(){
    	var tel = $("#tel").val();
		var vcode = $("#vcode").val();
		var url ="/login";
		var postStr = 'ajax=1&username=' + tel +'&password=' + vcode +'&remember-me=on';
		
		if(tel=="" || vcode==""){
			popMsg("请填写手机号和验证码", 'info');
		}else{
	   		$.ajax({
	   			url:url,
	   			method: "POST",
	   			data:postStr,
	   			success: function(data){
	   				loginSucc = data.success;
	   				if(loginSucc){
	   					$("#loading").css("display","block");
	   					var hasmobile = window.localStorage.getItem("mobile");
	   					if(hasmobile){
	   						window.localStorage.removeItem("mobile");
	   					}
	   					window.localStorage.setItem("mobile",tel);
	   					
	   					if(linkPage && linkPage != null){
	   						window.location.href = linkPage;
	   					}else{
	   						window.location.href = "/self_service/index";
	   					}
	   				}else{
	   					popMsg('手机号或验证码错误', 'info');
	   				}
	   			},
	   			error: function(){
	   				popMsg("系统异常","danger");
	   			}
	   		})
		}
    }
```

