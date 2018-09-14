# 内置指令

## v-for索引问题

```javascript
<div v-for = "(item, index) in item" v-on:click = "showItem(index)">
  //先item,在index.            $index貌似不适用于Vue
```

## JS





## $emit（暂时粗浅的理解）

```javascript
this.$emit("事件名",arg);                 //触发当前实例上的事件  arg:值
this.$parent.$emit("事件名",arg);         //子组件通知父组件发出的事件
this.$on("事件名",function(){
  //监听到这个事件后运行的函数.
})
```



