---
title: ES6的常用新特性
date: 2017-09-13 22:21:18
tags: [Javascript, 前端]
summary: ECMAScript 6（以下简称ES6）是JavaScript语言的下一代标准。因为当前版本的ES6是在2015年发布的，所以又称ECMAScript 2015。虽然目前并不是所有浏览器都能兼容ES6全部特性，但越来越多的程序员在实际项目当中已经开始使用ES6了。
---
(搬运于[segmentfault](https://segmentfault.com/a/1190000004365693) 作者：[zach5078](https://segmentfault.com/u/zach5078))
这些是最常用的几个语法`let, const, class, extends, super, arrow functions, template string, destructuring, default, rest arguments`

---
ECMAScript 6（以下简称ES6）是JavaScript语言的下一代标准。因为当前版本的ES6是在2015年发布的，所以又称ECMAScript 2015。虽然目前并不是所有浏览器都能兼容ES6全部特性，但越来越多的程序员在实际项目当中已经开始使用ES6了。
<!-- more -->
### let, const
`let`为javaScript新增了块级作用域。用它声明的变量，只在`let`命令所在的代码块内有效。
```
let name = 'zach'

while (true) {
    let name = 'obama'
    console.log(name)  //obama
    break
}

console.log(name)  //zach
```

---
`const`用来声明常量。一旦声明，常量的值就不能改变。
```
const PI = Math.PI

PI = 23 //Module build failed: SyntaxError: /es6/app.js: "PI" is read-only
```
当我们尝试去改变用const声明的常量时，浏览器就会报错。
const有一个很好的应用场景，就是当我们引用第三方库的时声明的变量，用const来声明可以避免未来不小心重命名而导致出现bug：
```
const monent = require('moment')
```

---
### class,extends,super
ES6引入了`Class（类）`这个概念。新的class写法让对象原型的写法更加清晰、更像面向对象编程的语法，也更加通俗易懂。
```
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        console.log(this.type + ' says ' + say)
    }
}

let animal = new Animal()
animal.says('hello') //animal says hello

class Cat extends Animal {
    constructor(){
        super()
        this.type = 'cat'
    }
}

let cat = new Cat()
cat.says('hello') //cat says hello
```
上面代码首先用class定义了一个“类”，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。简单地说，constructor内定义的方法和属性是实例对象自己的，而constructor外定义的方法和属性则是所有实例对象可以共享的。

**Class之间可以通过`extends`关键字实现继承。**
上面定义了一个Cat类，该类通过extends关键字，继承了Animal类的所有属性和方法。

---

`super`关键字，它指代父类的实例（即父类的this对象）。
子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。

---

### arrow function
**`arrow箭头函数`**
```
function(i){ return i + 1; } //ES5
(i) => i + 1 //ES6
```
如果方程比较复杂，则需要用`{}`把代码包起来：
```
//ES5
function(x, y) { 
    x++;
    y--;
    return x + y;
}

//ES6
(x, y) => {x++; y--; return x+y}
```
除了看上去更简洁以外，arrow function还有一项超级无敌的功能！
长期以来，JavaScript语言的this对象一直是一个令人头痛的问题，在对象方法中使用this，必须非常小心。例如：

```
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout(function(){
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}

 var animal = new Animal()
 animal.says('hi')  //undefined says hi
 ```
 运行上面的代码会报错，这是因为setTimeout中的this指向的是全局对象。所以为了让它能够正确的运行，传统的解决方法有两种：

1.第一种是将this传给self,再用self来指代this
```
   says(say){
       var self = this;
       setTimeout(function(){
           console.log(self.type + ' says ' + say)
       }, 1000)
```
2.第二种方法是用bind(this),即
```
   says(say){
       setTimeout(function(){
           console.log(this.type + ' says ' + say)
       }.bind(this), 1000)
```
但现在我们有了箭头函数，就不需要这么麻烦了：
```
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout( () => {
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}
 var animal = new Animal()
 animal.says('hi')  //animal says hi
```
当我们使用箭头函数时，函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，它的this是继承外面的，因此内部的this就是外层代码块的this。

---

### template string
```
$("#result").append(
  "There are <b>" + basket.count + "</b> " +
  "items in your basket, " +
  "<em>" + basket.onSale +
  "</em> are on sale!"
);
```
我们要用一堆的'+'号来连接文本与变量，而使用ES6的新特性模板字符串``后，我们可以直接这么来写：
```
$("#result").append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```
**用反引号（\`)来标识起始，用${}`来引用变量**，而且所有的空格和缩进都会被保留在输出之中，是不是非常爽？！
### Destructuring解构
**ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（`Destructuring`）。**
```
//ES5
let cat = 'ken'
let dog = 'lili'
let zoo = {cat: cat, dog: dog}
console.log(zoo)  //Object {cat: "ken", dog: "lili"}

//ES6
let cat = 'ken'
let dog = 'lili'
let zoo = {cat, dog}
console.log(zoo)  //Object {cat: "ken", dog: "lili"}

//反过来可以这么写：
let dog = {type: 'animal', many: 2}
let { type, many} = dog
console.log(type, many)   //animal 2


```

---

### Default,rest
**`default`很简单，意思就是默认值。**
大家可以看下面的例子，调用animal()方法时忘了传参数，传统的做法就是加上这一句type = type || 'cat' 来指定默认值。
```
function animal(type){
    type = type || 'cat'  
    console.log(type)
}
animal()
```
ES6可以这么写：
```
function animal(type = 'cat'){
    console.log(type)
}
animal()
```

---

**`rest参数`**
* est参数（形式为“...变量名”） *
* 用于获取函数多余参数，将多余参数放入数组中。 *
```
function animals(...types){
    console.log(types)
}
animals('cat', 'dog', 'fish') //["cat", "dog", "fish"]
```

---
### import  export
**ES6的module功能**
假设我们有两个js文件: index.js和content.js,现在我们想要在index.js中使用content.js返回的结果
```
//index.js
import animal from './content'

//content.js
export default 'A cat'
```
`export`命令除了输出变量，还可以输出函数，甚至是类（react的模块基本都是输出类）
```
//index.js

import { say, type } from './content'  
let says = say()
console.log(`The ${type} says ${says}`)  //The dog says Hello

//index.js

import { say, type } from './content'  
let says = say()
console.log(`The ${type} says ${says}`)  //The dog says Hello
```
这里输入的时候要注意：大括号里面的变量名，必须与被导入模块（content.js）对外接口的名称相同。

---
####  修改变量名
此时我们不喜欢type这个变量名，因为它有可能重名，所以我们需要修改一下它的变量名。在es6中可以用as实现一键换名。
```
//index.js

import animal, { say, type as animalType } from './content'  
let says = say()
console.log(`The ${animalType} says ${says} to ${animal}`)  
//The dog says Hello to A cat

```

---
#### 模块的整体加载
除了指定加载某个输出值，还可以使用整体加载，即用星号（*）指定一个对象，所有输出值都加载在这个对象上面。
```
//index.js

import animal, * as content from './content'  
let says = content.say()
console.log(`The ${content.type} says ${says} to ${animal}`)  
//The dog says Hello to A cat
```
通常星号*结合as一起使用比较合适。


