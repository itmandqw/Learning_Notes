# 数组

## filter

filter：<https://www.cnblogs.com/cjx-work/p/8052865.html>

所有数组方法详细：<https://www.jb51.net/article/87930.htm>

用于把`Array`的某些元素过滤掉，然后返回剩下的元素。

和`map()`类似，`Array`的`filter()`也接收一个函数。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`true`还是`false`决定保留还是丢弃该元素。（为true时保留。）

```javascript
var arr = [1,2,3,4,5,6];
var res = arr.filter(function(x){     //返回数组中的奇数
  return x % 2 !== 0;
})
console.log(res);          //[1,3,5]
arr.filter(element,index,self)的回调函数有三个参数，通常我们只使用第一个
var arr = ['A', 'B', 'C'];
element // A B C
index   //0 1 2 
self   //就是变量arr本身
//利用filter实现数组去重
var arr =["apple","orange","banner","apple","orange","pear","waterml"];
var res = arr.filter(function(element,index,self){
  return self.indexOf(element) === index;
})
console.log(res);     //["apple","orange","banner","pear","waterml"]
//这种方法依靠的是indexOf总是返回第一次元素的位置，后续重复的元素索引与之对应不上被过滤掉。
```

# map

map方法会迭代数组中的每一个元素，并根据回调函数来处理每一个元素。最终返回一个新数组，但不会改变原始数组(相当于把原始数组复制了一遍)。eg:

```javascript
var arr = [1,2,3,4];
var newArr = arr.map(function(item){
  return item*10;
})
console.log(nerArr);         //[10,20,30,40]
//回调函数同样支持三个参数，同filter。
```

# `forEach`

`forEach`是原生JS的方法，与map等方法不同的是，`forEach`没有返回值。

```javascript
//1 没有返回值
arr.forEach((item,index,array)=>{
    //执行代码
})
//参数：item数组中的当前项, index当前项的索引, array原始数组；
//数组中有几项，那么传递进去的匿名回调函数就需要执行几次；
//理论上这个方法是没有返回值的，仅仅是遍历数组中的每一项，不对原来数组修改；但是可以自己通过数组的索引来修改原来的数组；

var ary = [12,23,24,42,1];  
var res = ary.forEach(function (item,index,ary) {  
       ary[index] = item*10;  
})  
console.log(res);//--> undefined; 没有返回值的体现 
console.log(ary);//--> 通过数组索引改变了原数组；  [120, 230, 240, 420, 10]
```

## every()/some()

every()是对数组中每一项运行给定函数，如果该函数对**每一项**返回true,则返回true。

some()是对数组中每一项运行给定函数，如果该函数对**任一项**返回true，则返回true。

```javascript
var arr = [1,2,3,4,6,5];
var res1 = arr.every(function(item,index,ary){       //同上
  return item > 3
})
var res2 = arr.some(function(item,index,ary){       //同上
  return item > 3
})
console.log(res1,res2)         //false,true
```

***

## slice

slice()：返回从原数组中指定开始下标到结束下标之间的项组成的新数组。slice()方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下， slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。

```javascript
var arr = [1,3,5,7,9,11];
var arrCopy = arr.slice(1);
console.log(arrCopy); //[3, 5, 7, 9, 11]
var arrCopy2 = arr.slice(1,4);
console.log(arrCopy2); //[3, 5, 7]
var arrCopy3 = arr.slice(1,-3);
console.log(arrCopy3); //[3, 5]
var arrCopy4 = arr.slice(-4,-1);
console.log(arrCopy4); //[5, 7, 9]
console.log(arr); //[1, 3, 5, 7, 9, 11](原数组没变)

arrCopy只设置了一个参数，也就是起始下标为1，所以返回的数组为下标1（包括下标1）开始到数组最后。 
arrCopy2设置了两个参数，返回起始下标（包括1）开始到终止下标（不包括4）的子数组。 
arrCopy3设置了两个参数，终止下标为负数，当出现负数时，将负数加上数组长度的值（6）来替换该位置的数，因此就是从1开始到4（不包括）的子数组。 
arrCopy4中两个参数都是负数，所以都加上数组长度6转换成正数，因此相当于slice(2,5)。
```

## splice

splice()：很强大的数组方法，它有很多种用法，可以实现删除、插入和替换。

删除：可以删除任意数量的项，只需指定 2 个参数：要删除的第一项的位置和要删除的项数。例如， splice(0,2)会删除数组中的前两项。

插入：可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、 0（要删除的项数）和要插入的项。例如，splice(2,0,4,6)会从当前数组的位置 2 开始插入4和6。
替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice (2,1,4,6)会删除当前数组位置 2 的项，然后再从位置 2 开始插入4和6。

splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项，如果没有删除任何项，则返回一个空数组。

```javascript
var arr = [1,3,5,7,9,11];
var arrRemoved = arr.splice(0,2);
console.log(arr); //[5, 7, 9, 11]
console.log(arrRemoved); //[1, 3]
var arrRemoved2 = arr.splice(2,0,4,6);
console.log(arr); // [5, 7, 4, 6, 9, 11]
console.log(arrRemoved2); // []
var arrRemoved3 = arr.splice(1,1,2,4);
console.log(arr); // [5, 2, 4, 4, 6, 9, 11]
console.log(arrRemoved3); //[7]
```

**区别：**splice会改变原数组，而slice不会改变原数组。