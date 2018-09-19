## typeof 和instanceof的区别

```javascript
typeof 一般只能返回如下几个结果："number"、"string"、"boolean"、"object"、"function" 和 "undefined"。
在javascript中，判断一个变量的类型常常会用typeof运算符，但在使用typeof运算符判断引用类型的值会出现问题。无论引用的是什么类型的对象，它都返回"Object",例如Array,Null等。这也是`typeof`的局限性。

对于这些引用类型，我们可以用instanceof来检测：
instanceof运算符用来测试一个对象在其原型链中是否存在一个构造函数的prototype属性。
语法： Object.instanceof constructor
eg: var a = new Array();
console.log(a instanceof Array )                //true
```

***

##  `__proto__` 和prototype区别：

**`__proto__`是每个对象都有的一个属性，而prototype是函数才会有的属性!!!** 

`__proto__`

对象具有属性`__proto__`，可称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型，这也保证了实例能够访问在构造函数原型中定义的属性和方法。

```javascript
function Foo(){}
var Boo = {name: "Boo"};
Foo.prototype = Boo;
var f = new Foo();

console.log(f.__proto__ === Foo.prototype); // true
console.log(f.__proto__ === Boo);   // true
Object.getPrototypeOf(f) === f.__proto__;   // true
```

`prototype`

prototype是通过调用构造函数而创建的那个对象实例的原型对象。`hasOwnProperty()`判断指定属性是否为自有属性；in操作符对原型属性和自有属性都返回true。

```javascript
var obj = {a: 1};
obj.hasOwnProperty("a"); // true
obj.hasOwnProperty("toString"); // false
"a" in obj; // true
"toString" in obj; // true

//鉴别原型属性：
function hasPrototypeProperty(obj, name){
    return name in obj && !obj.hasOwnProperty(name);
}
```

Javascript中所有的对象都是Object的实例，并继承Object.prototype的属性和方法，也就是说，Object.prototype是所有对象的爸爸

在对象创建时，就会有一些预定义的属性，其中定义函数的时候，这个预定义属性就是prototype,这个prototype是一个普通的对象。

而定义普通的对象的时候，就会生成一个`__proto__`，这个`__proto__`指向的是这个对象的构造函数的prototype.



方法(Function)方法这个特殊的对象，除了和其他对象一样有上述`__proto__`属性之外，还有自己特有的属性——原型属性（prototype），这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做constructor，这个属性包含了一个指针，指回原构造函数。

而最终的最终`__proto__`指向null

由此想到了原型链：

原型链是针对构造函数的，比如我先创建了一个函数，然后通过一个变量new了这个函数，那么这个被new出来的函数就会继承创建出来的那个函数的属性，然后如果我访问new出来的这个函数的某个属性，但是我并没有在这个new出来的函数中定义这个变量，那么它就会往上（向创建出它的函数中）查找，这个查找的过程就叫做原型链。通过`__proto__`向上查找。类似于CSS中的继承一样。如果自身没有定义就会继承父元素的样式（属性和方法）;

Object  ==>  构造函数1   ==>   构造函数2...

***

## 作用域链和原型链

原型链见上↑

首先要明白：作用域是针对于变量的。

比如我们创建了一个函数，函数里面又包含了一个函数，那么现在就有三个作用域

全局作用域==>函数1作用域==>函数2作用域

```javascript
var a = 1;       //全局作用域
function b(){
  	var a = 2;
  	function c(){
  		var a = 3;
        console.log(a)
	}
  c();
}
b(); //3
//执行函数c（）的时候它在自己的范围内找到了变量 a 所以就不会越上继续查找，如果在函数c()中没有找到则会继续向上找，一直会找到全局变量a，这个查找的过程就叫作用域链。
//注：作用域只会向上查找并且作用域链是单向的。针对在这个例子可以理解为：
//函数c==> 函数b==> 全局作用域
```

全局window是最大的作用域，全局里声明的函数是次一级的作用域，这样的函数里面可以访问到全局里的各种变量和函数，但在全局中访问不到这个函数里面的函数和变量；这样次一级里面的函数是再次一级，它同样也是可以访问上一级和全局的函数与变量，而上级访问不到这个里面的东西，这样一层一层的递进，就成了一条链子，也就是作用域链

































