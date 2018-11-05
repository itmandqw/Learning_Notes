## 常用的正则表达式修饰符

```javascript
i 不区分 (ignore) 大小写
eg: /abc/i 可以匹配abc,ABc,AbC,aBC等
```

```javascript
g 全局（global）匹配
如果不带g,正则匹配过程中字符串从左到右匹配，找到第一个符合条件的即匹配成功，返回
如果带g,则字符串从左到右，找到每个符合条件的都记录下来,直到字符串结尾位置
eg: 
var str = 'aaaaaaa',
var reg1 = /a/     
str.match(reg1) //["a", index: 0, input: "aaaaaaa", groups: undefined]
var reg2 = /a/g
str.match(reg2)   //["a", "a", "a", "a", "a", "a"]
```

```javascript
m 多行(more)匹配
若存在换行 \n 并且有开始 ^ 或结束 $ 符 的情况下，和g一起使用实现全局匹配
g默认只匹配第一行，添加m之后实现匹配多行，每个换行符之后就是开始。
eg:
var str = "abcggab\nnaddab";
var reg1 = /^abc/gm;   str.match(reg1)    //["abc"]
var reg2 = /ab$/gm;    str.match(reg2)    //["ab","ab"]
```





## 常用的正则表达式方法

## `test`

检查字符串是否匹配RegExp，返回值true或false

```javascript
var reg = /a/;
var str = "dsadadsba";
console.log(reg.test(str));            //true
```

## `exec`

查找并返回当前匹配的结果，并以数组的形式返回。如果匹配不到，则返回null

```javascript
var str = "lalblc";
var reg = new RegExp("l.","");  //.表示l后面有一个字符，两个.代表两个字符，以此类推
var arr = reg.exec(str);  //["la", index: 0, input: "lalblc", groups: undefined]
arr有三个属性：
第一项为匹配到的字符串
index: 当前匹配项的位置;
input: 源字符串
//exec 方法受参数 g 的影响。若指定了 g，则下次调用 exec 时，会从上个匹配的 lastIndex 开始查找,但仍每次返回一个匹配项,exec是RegExp的方法,所以str为参数
```

### `match`

```javascript
//match是字符串的方法，所以reg为参数
var str = "lalblc";
var reg = new RegExp("l.","");
var arr = str.match(reg); //["la", index: 0, input: "lalblc", groups: undefined]
如果指定参数g,即             //reg = new RegExp("l.","g");
则一次性返回所有的匹配结果    // ["la", "lb", "lc"]
```

 ## exec 和 match 的区别：

1，exec是正则表达式的方法，而不是字符串的方法，它的参数是字符串。

2，match是字符串的方法，它的参数是正则表达式。

3，exec和match返回的都是数组(正则表达式无子表达式(正则嵌套)，并且定义非全局匹配)，此时两者返回的数组一致 `["la", index: 0, input: "lalblc", groups: undefined]`。

4，全局匹配，且正则表达式没有子表达式的时候

match返回所有匹配项组成的一个数组：`["la", "lb", "lc"]`

而exec找到第一个匹配即返回: ``["la", index: 0, input: "lalblc", groups: undefined]`。`

5，不做累述。没有意义

```javascript
//总结：exec与全局是否定义无关系，而match则于全局相关联，当定义为非全局，两者执行结果相同
```



## 正则表达式对象定义方法

1、构造函数定义

```javascript
new RegExp(pattern,attributes);
eg: var reg = new RegExp("abc","g")      //g,i,m
```

2、文本定义

```javascript
var reg = /abc/g;       //作用同上
```

​	