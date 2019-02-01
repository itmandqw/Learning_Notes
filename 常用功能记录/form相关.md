# 根据表单Id和数据自动填充表单

```javascript
//e: 表单Id
//t: 数据源，数据源的key应与表单中的name属性一致方可填充
function editFormByFormId(e,t){//根据表单Id填充表单
	var m="#"+e;
	var n=$(m).serializeArray();
	$.each(n, function(){
		var a=this.name;
		if($(m + " [name="+a+"]")[0].tagName!="SELECT"){
			$(m + " [name="+a+"]").val("");
		}
		if(t.hasOwnProperty(a)){
			if(a.indexOf("Date")>=0 || a.indexOf("Time")>=0){
				$(m + " [name="+a+"]").val(Ts.date_1(t[a]));
			}else if (a.className == 'select') {
                $(m + " [name="+a+"]").value(t[a]);
            }else{
				$(m + " [name="+a+"]").val(t[a]);
			}
			
		}
	})
}
```

# 检查表单里的必填项是否填写完整

```javascript
function checkFrom(e){//检查form表单是否存在必填项(表单必须要有name属性)
	var m="#"+e;
	var n=$(m).serializeArray();
	var c="";
	$.each(n, function(){
		var a=this.name;
		var b=this.value;
		var an=$("[name="+a+"]").attr("required");
		if(an=="required" && b==""){
			c=a;
			return false
		}
	})
	return c;       //将首个未填写的必填项的name返回出来
}
```

# 清空表单

```javascript
function initFrom(e){//清空表单
	var m="#"+e;
	var n=$(m).serializeArray();
	$.each(n, function(){
		var a=this.name;
		if($("[name="+a+"]")[0].tagName != "SELECT"){
			$("[name="+a+"]").val("");
		}
	})
}
```

