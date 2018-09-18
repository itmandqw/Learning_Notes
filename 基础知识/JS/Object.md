# 创建对象的几种方法以及优缺点总结：

1.使用字面量方式或通过Object构造函数创建单个对象

```javascript
//（1）字面量方式
var obj = {
  name: "xiaoming",
  age: 18
}
//(2)Object关键字方式
var obj = new Object();
obj.name = "xiaohong";
obj.age = 18;
//对象字面量是对象定义的一种简写形式，目的在于简化创建包含大量属性的对象的过程。也就是说，第一种和第二种方式创建对象的方法其实都是一样的，只是写法上的区别不同
//缺点：会产生大量的重复代码，不适合量产
```

2.工厂模式：

```javascript
function createPerson(name,age){
  var o = new Object();
  o.name = name;
  o.age = age;
  o.sayName = function(name){
  	console.log(this.name);
}
  return o;
}
//可以无数次调用这个工厂函数，每次都会返回一个包含两个属性和一个方法的对象
//缺点：返回的是一个对象，无法解决对象识别问题，即不能知道返回的对象的类型。
```

3.构造函数方式：

```javascript
function Person(name,age){
  this.name = name;
  this.age = age;
  this.sayName = function(name){
  	console.log(this.name);
  }
}
var person1 = new Person("xioaming",18)
//按照惯例，构造函数始终要应该以一个大写字母开头，而非构造函数则应该以一个小写字母开头。
//使用new关键字来调用这个函数，直接将属性和方法赋给了this对象
//缺点：使用构造函数创建的对象，每个方法都要在每个实例上重新创建一次。就会占用很多不必要的内存.
```

4.原型模式：

```javascript
function Person(){  
}
Person.prototype.name = "xiaoming";
Person.prototype.age = 18;
Person.prototype.sayName = function(){  
  console.log(this.name);
}
 var person1 = new Person();
 person1.name = "langlang" ==>可以修改替换成自己的专有属性而不影响原型中的值。
 前提是修改替换的不是引用类型。
 console.log(person1.name)    //langlang 
//缺点: 使用原型，所有的属性和方法都将被共享，这是个很大的优点，同时也会带来一些缺点。原型中所有属性实例是被很多实例共享的，这种共享对于函数非常合适。对于那些包含基本值的属性也勉强可以，毕竟实例属性可以屏蔽原型属性。但是引用类型值，就会出现问题了eg：
function Person() { 	
} 
Person.prototype = {  
  name: 'jiang',  
  friends: ['Shelby', 'Court'] 
} 
var person1 = new Person() 
var person2 = new Person() 
person1.friends.push('Van') 
console.log(person1.friends) //["Shelby", "Court", "Van"] 
console.log(person2.friends) //["Shelby", "Court", "Van"] 
console.log(person1.friends === person2.friends) // true
//即改变一个共享的引用值会影响到其他实例的值。
```

5.构建函数+原型组合模式：

这是使用最为广泛、认同度最高的一种创建自定义类型的方法。它可以解决上面那些模式的缺点。使用此模式可以让每个实例都会有自己的一份实例属性副本，但同时又共享着对方法的引用这样的话，即使实例属性修改引用类型的值，也不会影响其他实例的属性值了。

```javascript
function Person(name) { 
 this.name = name 
 this.friends = ['Shelby', 'Court'] //引用类型
} 
Person.prototype.sayName = function() {  //共用的方法
 console.log(this.name) 
} 
var person1 = new Person() 
var person2 = new Person() 
person1.friends.push('Van') 
console.log(person1.friends) //["Shelby", "Court", "Van"] 
console.log(person2.friends) // ["Shelby", "Court"] 
console.log(person1.friends === person2.friends) //false
```

6.动态原型模式：

动态原型模式将所有信息都封装在了构造函数中，初始化的时候，通过检测某个应该存在的方法时候有效，来决定是否需要初始化原型

```javascript
function Person(name, job) { 
  // 属性 
 this.name = name 
 this.job = job 
 // 方法 
 if(typeof this.sayName !== 'function') { 
  Person.prototype.sayName = function() { 
    console.log(this.name) 
  } 
 } 
} 
var person1 = new Person('Jiang', 'Student') 
person1.sayName()
//只有在sayName方法不存在的时候，才会将它添加到原型中。这段代码只会初次调用构造函数的时候才会执行。
//这里对原型所做的修改，能够立即在所有实例中得到反映
//其次，if语句检查的可以是初始化之后应该存在的任何属性或方法，所以不必用一大堆的if语句检查每一个属性和方法，只要检查一个就行
```



