---
title: JavaScript高级程序设计读书笔记
date: 2018-07-18 21:05:01
tags: [前端,读书笔记]
---

## 第一章 JavaScript简介
#### JavaScript由 ECMAScript、DOM、BOM三部分组成。
  ECMAScript，由ECMA0262定义，提供核心语言功能。
  文档对象模型（DOM，Document Object Model）是针对XML但经过扩展用于HTML的应用编程接口（API，Application Programming Interface）。
  浏览器对象模型（BOM，Browser Object Model）提供与浏览器交互的方法和接口。

## 第二章 在HTML中使用JavaScript
###  < script >元素 
####  使用< script >元素的方式 
  ①在页面中嵌入JavaScript代码
    ```html
    <script type=text/javascript>
      function sayHi(){
        alert("Hi");
      }
    </script>
    ```
  ②包含外部JavaScript文件。
    ```html
    <script type="text/javascript"  src="example.js"></script>
    ```
  _ 注意：带有src属性的script元素如果包含了嵌入的代码，则只会下载并执行外部脚本文件，嵌入的代码会被忽略。 _ 

####  标签的位置 
  传统做法中会将< script >元素全部放在页面的< head >元素中
  但是这意味着全部的JavaScript代码被下载解析执行完毕后才能开始呈现页面内容。将会导致页面在加载页面之前会有些许延迟，为了避免这个问题，** 一般将JavaScript代码放置在< body >元素中页面内容的后面。**

####  嵌入代码与外部文件  
  一般认为最好的做法还是尽可能的是用外部文件来包含JavaScript代码。
  其有可维护，可缓存等优点
<!-- more -->
## 第三章 基本概念
### 语法
#### 区分大小写
  ECMAScript中的一切（变量、函数名和操作符）都区分大小写。
#### 标识符
  ** 就是指变量、函数、属性的名字、或者函数的参数。**
  标识符可以是按照以下规则组合起来的一个或多个字符：
  + 第一个字符必须是一个字母、下划线(_)或者一个美元符号($)；
  + 其他字符可以是字母、下划线、美元符号或数字。
  ECMAScript标识符采用**驼峰大小写**格式，也就是第一个字母小写，剩下的每个单词的首字母大写。例如：
  doSomethingImportant

#### 注释
  单行注释
    ```js
      //单行注释
    ```
  
  块级注释 
    ```js
      /*
      *   这是一个多行
      *   （块级）注释
      */
    ```
#### 严格模式
  ECMAScript 5引入了严格模式（strict mode）的概念。
  启用严格模式，需在代码顶部添加如下代码：
  ```js
    "use strict"
  ```
  在函数内部的上方包含这条编译知识，也可以指定函数在严格模式下执行：
  ```js
    function doSomething(){
      "use strict";
      //函数体
    }
  ```
#### 语句
  ECMAScript中的语句以一个分好结尾；如果省略分好，则界学习期确定语句的结尾。
  ```js
    var sum = a + b     //即使没有分号也是有效的语句---不推荐
    var sum = a + b;    //有效的语句---推荐
  ```
  在控制语句中使用代码块
  ```js
    if(test)
      alert(test);    //有效但容易出错，不要使用

    if(test){
      alert(test);    //推荐使用
    }
  ```
#### 关键字和保留字
  关键字：
   break  do  instanceof  typeof
   case else new var
   catch finally return void
   continue for switch while
   debuger function this with
   default if throw
   delete in try
  保留字：
  class enum extends super
  const export inport

#### 变量
  ECMAScript变量是松散型（可以用来保存任何类型的数据）的。
  定义变量时要使用var操作符，后跟变量名：
  ```js
    var message
  ```
  也可以直接初始化
  ```js
    var message = "hi";
  ```
  _使用var操作符定义的变量将成为定义该变量的作用域中的局部变量_
  同时定义多个变量：
  ```js
    var message = "hi",
          found = false,
          age = 29;
  ```
#### 数据类型
  ECMAScript中有 ** Undefined、Null、Boolean、Number、String ** 五种**基本数据类型**，还有一种复杂数据类型--Object,** Object本质上是由一组无需的名值对组成的 **

  ** typeof操作符 **

  用来检测变量的数据类型
  ```js
    var message = "some string";
    alert(typeof message);      //"string"
    alert(typeof 90);           //"number"
  ```
  _注意：调用typeof null会返回“object”，因为特殊值null被认为是一个空的对象引用。_

#### Undefined类型
  Undefined类型只有一个值，即特殊的Undefined。
  未经初始化的值默认就会取得Undefined值。
  ```js
    var messsage;
    alert(message == undefined);  //true
  ```
#### Null类型
  Null类型只有一个值，即特殊的Null。
  Null的值表示一个空对象指针
  如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null而不是其他值。

#### Boolean类型
  Boolean有两个值，true和false.
  Boolean()函数可以将一个值转换为其对应的Boolean值，如：
  ```js
    var message = "hello";
    var messageAsBoolean = Boolean(message);
  ```
#### Number类型
  Number类型使用IEEE754格式来表示整数和浮点数。

  isFinite()函数在参数位于最小与最大数值之间时会返回true。

  NaN，即非数值（Not a Number）用于表示一个本来要返回数值的操作数未返回数值的情况。
  NaN有两个特点
    ①任何涉及NaN的操作都会返回NaN
    ②NaN与任何值都不对等，包括NaN本身。
  isNan()函数接收一个参数，这个方法可以确定这个参数是否“不是数值”， _isNaN()在接收到一个值后，会尝试将这个值转换为数值，不能转化为数值的值会返回true。 _

  ** 数值转换 **
  有3个函数可以把非数值转换为数值：
    ①Number()       //用于任何数据类型
    ②parseInt()     //专门用于把字符串转换为数值
    ③parseFloat()   //专门用于把字符串转换为数值
  Number()  转换规则：
  + 如果是Boolean值，true和false将分别转换为1和0.
  + 如果是数字值，只是简单的传入和传出。
  + 如果是null值，返回0.
  + 如果是Undefined，返回NaN。
  + 如果是字符串，遵循下列规则
    + 如果字符串中只包含数字，则将其转换为十进制数值，即"123"会变成123，"011"会变成11（注意:前导的0被忽略了）。
    + 如果字符串中包含有效的浮点格式，如"1.1",则将其转换为对应的浮点数值（同样前导的0也会被忽略）。
    + 如果字符串中包含有效的十六进制格式，如"0xf"，则将其转化为相同大小的十进制数值。
    + 空字符串，转换为0。
    + 如果字符串中包含除以上格式之外的字符，则将其转换为NaN。
  + 如果是对象，则调用对象的valueOf()方法，然后再一次按照前面的规则转换返回的值。如果转换的结果是NaN，则调用toString()方法，然后再依次按照前面的规则转换发挥的字符串值。

parseInt() 转换规则
  parseInt()转换字符串时，会忽略字符串前面的空格，直至找到第一个非空格字符。
  如果第一个字符不是数字字符或者负号，parseInt()会返回NaN；
  如果第一个字符是数字字符，parseInt()会继续解析第二个字符，直到解析完所有后续字符或者遇到了一个非数字字符。
  ```js
    var num1 = parseInt("1234blue");    //1234
    var num2 = parseInt("");            //NaN
    var num3 = parseInt("22.5")         //22
  ```
  parseInt()也可以识别出各种整数格式（八进制、十六进制、二进制），如果知道要解析的值转换的基数（即多少进制），那么需要指定对应的基数。
  ```js
    //var num = parseInt("字符串",对应的基数)
    var num = parseInt("0xAF",16);    //175
  ```

  parseFloat() 转换规则
  parseFloat()会从第一个字符开始解析每个字符，直至解析玩所有后续字符或者遇到一个无效的浮点数字位置。_也就是说字符串中的第一个小数点是有效的，而第二个小数点是无效的_
  parseFloat()只解析十进制


#### String类型
  String类型用于表示由零或多个16位Unicode字符组成的字符序列，即字符串。
  字符串可以由 ' 或 " 表示，要成对出现。

  ** 字符字面量 ** 
  String数据类型包含一些特殊的字符字面量，也叫_转移序列_，用于表示非打印字符，或者具有其他用途的字符。

  | 字面量    | 含义       |
  | -------- |:---------:|  
  | \n    | 换行    |
  | \t    | 制表    |
  | \b    | 空格    |
  | \r    | 回车    |
  | \f    | 进纸    |
  | ` \\ `  | 斜杠    |
  | \'    | 单引号（'），在用单引号的字符串中使用。例如：'He said, \'hey.\''|
  | \"    | 双引号（"），在用单引号的字符串中使用。例如："He said, \"hey.\""|

  ** 字符串的特点 **
    字符串是不可变的
  ** 转换为字符串 ** 
  toString()方法
  ```js
  var num = 10;
  alert(mun.toString());    //"10"
  ```
  toString()方法 转换规则
  + 如果值有toString()方法，则调用该方法（没有参数）并返回相应的结果；
  + 如果值是null,则返回"null"；
  + 如果值是undefined,则返回“undefined”。

#### Object类型

  ECMAScript中的对象其实就是一组数据和功能的集合。创建对象可以通过执行new操作符后跟要创建对象类型的名称来创建

  ```js
    var o = new Object();
  ```
  
在ECMAScript中object类型是所有它的实例的基础。
  object每个实例都具有以下属性和方法
  + constructor：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数(constructor)就是Object()。
  + hasOwnProperty(propertyName):用于检查给定的属性在当前对象实例中是否存在。其中，作为参数的属性名(propertyName)必须以字符串形式指定(例如：o.hasOwnProperty("name"))。
  + isPrototypeOf(object):用于检查传入的对象是否是传入对象的原型。
  + propertyIsEnumerable(propertyName)用于检查给定的属性是否能够使用for-in语句来枚举，作为参数的属性名必须以字符串形式指定。
  + toLocaleString():返回对象的字符串表示，该字符串与执行唤醒的地区对应。
  + toString():返回对象的字符串表示。
  + valueOf():返回对象的字符串、数值或布尔值表示。


### 操作符
#### 一元操作符
  只能操作一个值的操作符叫一元操作符。
 **递增递减操作符**

  前置型：

  ```js
    var age =29;
        ++age;      //= age = age + 1;
  ```
  前置递增递减操作时，变量的值都是在语句中被求值以前改变的。
  ```js
  var age =29;
  var anotherAge = --age + 2;
  alert(age);     //输出28
  alert(anotherAge);      //输出30
  ```
  这个例子中变量anotherAge的初始值等于变量的值前置递减之后机上2。由于先执行了减法操作，age的值变成28，所以在加上2的结果就是30。

  后置型：

  ```js
  var age = 29;
    age++;          //age = age + 1;
  ```

** 后置递增递减与前置递增递减的区别，即递增和递减操作是在包含它们的语句被求值之后才执行的。 **

** 相等操作符 ** 
  == != 相等和不相等---先转换操作数（通常称为**强制转型**）再比较
  === !== 全等和不全等---仅比较不转换

  相等和不相等操作在转换不同数据类型时遵循以下基本规则：
  + 如果有一个操作数是布尔值，则在比较相等性之前先将其转换为数值--false转换为0，true转换为1；
  + 如果有一个操作数是字符串，另一个操作符是数值，在比较相等性之前先将字符串转换为数值；
  + 如果有一个操作数是对象，另一个操作数不是，则调用对象的valueOf()方法，用得到的基本类 型值按照前面的规则进行比较；
  + 这两个操作符在进行比较时要遵循以下规则：
    + null和Undefined是相等的。
    + 要比较相等性之前，不能将null和Undefined转换成其他任何值。
    + 如果有一个操作数是NaN，则相等操作符返回false，而不相等操作符返回true
    + 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回true；否则返回false。

** 条件操作符 ** 
```js
  var max = (num1 > num2) ? num1 : num2;
```

#### 语句
语句也称为流控制语句。

**if语句**

语法：
```js
if(condition) statement1 else staement2
```

** do-while语句**
do-while语句是一种后测试语句，在表达式求值之前，循环体内的代码至少会被执行一次。
语法：
```js
  do{
    statement
  }while(expression);
```

**while语句**
```js
  while(expression) statement;
```
**for语句**
for语句也是一种前测试循环语句，但它具有在执行循环之前初始化变量和定义循环后需要执行的代码的能力。
语法
```js
  for(initialization;expression;post-loop-expression) statement

  //实例
  var count = 10;
  var i;
  for(i = 0;i > count;i++){
    alert(i);
  }
```
for语句中的初始化表达式、控制表达式和循环后表达式都是可选的。

**for-in语句**
for-in语句是一种精准的迭代语句，可以用来枚举对象的属性。
```js
  for (property in expression) statement
```
```js
  for (var propName in window){
    document.write(propName);
  }
```
**label语句**
使用label语句可以在代码中添加标签，以便将来使用。一下是label语句的语法：
```js
  label: statement
```
**break和continue语句**
break和continue语句用于在循环中精确地控制代码的执行。
其中，break语句会立即退出循环，强制继续执行循环后面的语句。
而continue语句虽然也是退出循环，但是退出循环后会从循环的顶部继续执行。

**with语句**
with语句的作用是将代码的作用域设置到一个特定的对象中,语法：
```js
with(expression) statement
```
定义with语句的目的主要是为了简化多次编写同一个对象的工作。如下面的列子所示：
```js
  var qs = location.search.substring(1);
  var hostName = location.hostname;
  var url = location.href;
```
上面几行代码都包含location对象，如果使用location语句，可把上面的代码改成如下所示：
```js
with(location){
  var qs = search.substring(1);
  var hostName = hostname;
  var url = href;
}
```

**switch语句**
```js
switch (expression) {
  case value: statement
    break;
  case value: statement
    break;
  default: statement
}
```
switch语句中国的每一种情形(case)的含义是：“如果表达式等于这个值value，则执行后面的语句statement”。


**函数**

ECMAScript中的函数使用function关键字来声明，后跟一组参数以及函数体，基本语法如下所示：
```js
  fuZZnction functionName(arg0,arg1,...,argN){
    statements
  }`
```
实例：
```js
  function sayHi(name,message) {
    alert("hello" + name + "message");
  }
```

任何函数在任何时候都可以通过return语句后跟要返回的值来实现返回值。如下：
```js
  function sum(num1,num2) {
    return num1 + num2;
  }
```
位于return语句之后的任何代码都永远不会执行。如
```js
  function sum(num1,num2) {
    return num1 + num2;
    alert("hello world");  //永远不会被执行
  }
```

**理解参数**

在ECMAScript中的参数在内部使用一个数组来表示的。
函数体内可以通过arguments对象来访问这个参数数组，从而获取传递给函数的每一个参数。
在arguments的值永远与对应命名参数的值保持同步。它们的内存空间是相互独立的。
```js
  function doAdd(num1,num2) {
    arguments[1] = 10;
    alert(arguments[0] + num2);
  }
```
没有传递值的命名参数将自动被赋予Undefined值。

## 第四章 变量、作用域和内存问题
### 基本类型和引用类型的值
ECMAscript中的变量包含**基本类型值**和**引用类型值**。
基本类型值指的是简单的数据段。
引用类型值指那些可能由多个值构成的对象。

**动态的属性**
定义基本类型值和引用类型值是类似的：创建一个变量并为该变量赋值。对于引用类型的值，我们可以为其添加属性和方法，也可以改变和删除其属性和方法。
例子：
```js
var person = new Object();
person.name = "Nicholas";
alert(person.name);     //"Nicholas"
```
但是我们不用给基本类型的值添加属性。


**复制变量值**
从一个变量向另一个变量复制基本类型值，会在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上。
当从一个变量向另一个变量复制引用类型值时，同样也会将存储在变量对象中的值复制一份放到新变量分配的空间中。不同的是，**这个值的副本实际上是一个指针，而这个指针指向存储在堆中的一个对象。**复制结束后，两个变量实际上引用同一个对象。因此改变其中一个变量，就会影响另一个变量。
例子：
```js
  var obj1 = new Object();
  var obj2 = obj1;
  obj1.name = "Nicholas";
  alert(obj2.name);     //"Nicholas"
```

**传递参数** 
在ECMAScript中，所有函数的参数都是按值传递的。
在向参数传递基本类型的值时，被传递的值会被复制给一个局部变量（即命名参数）。
在向参数传递引用类型的值时，会把这个值在内存中的地址复制给一个局部变量，因此这个局部变量的变化会反映在函数的外部。

**检测类型**
typeOf操作符可以确定一个变量是字符串，数字，布尔值还是Undefined。如果变量的值是一个对象或者null，则typeOf会返回"object"。
检测引用类型是什么类型的对象需要用：instanceof操作符
语法：
```js
  result = variable instanceof constructor
```
如果变量是给定引用类型的实例。那么instanceof操作符就会返回true。
实例：
```js
alert(person instanceof instanceof Object);     //变量person是Object吗？
alert(colors instanceof instanceof Array);      //变量colors是Array吗？
alert(pattern instanceof instanceof RegExp);     //变量pattern是RegExp吗？
```
在检测一个引用类型值和Object构造函数时，instanceof始终会返回true。


**执行环境及作用域**
执行环境（execution context）定义了变量或函数有权访问的其他数据。
每个执行环境中都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。
全局执行环境是最外围的一个执行环境。在web浏览器中，全局执行环境被认为是window对象。
当代码在一个环境中被执行时，会创建变量对象的一个作用域链（scope chain），它的作用是保证对执行环境有权访问的所有变量和函数的有序访问。


```js
  var color = "blue";

  function changeColor(){
    if(color === "blue"){
      color = "red";
    }else{
      color = "blue";
    }
  }

  changeColor();
  alert("Color is now " + color);

```
在这个简单的例子中，函数changeColor()的作用域链包含了两个对象：它自己的变量对象（其中定义了auguments对象）和全局变量对象。 可以在函数内部访问变量color，就是因为可以在这个作用域链中找到它。
每个环境都可以向上搜索作用域链，以查询变量和函数名；但是任何环境都不能通过向下搜索作用域链从而进入另一个执行环境。

小结：
+ 基本类型值在内存中占据固定大小的空间，因此被保存在栈内存中；
+ 从一个变量向另一个变量复制基本类型的值，会创建这个值的副本；
+ 引用类型的值是对象，保存在堆内存中。
+ 从一个变量向另一个变量复制引用类型的值，复制的其实是指针，因此两个变量最终指向同一个对象。
+ 确认一个值是那种基本类型可以使用typeof操作符 ，而确定一个值是那种引用类型可以用instanceof操作符。
+ 所有变量（包括基本类型和应用类型）都存在于一个执行环境（也称为作用域）当中，这个执行环境决定了变量的生命周期，以及哪一部分代码可以访问其中的变量。以下是关于执行环境的几点总结：
  + 执行环境有全局执行环境（也称为全局环境）和函数执行环境之分；
  + 每次进入一个新执行环境，都会创建一个用于搜索变量和函数的作用域链；
  + 函数的局部环境不仅有权访问函数作用域中的变量，而且有权访问其包含（父）环境，乃至全局环境。
  + 全局环境只能访问全局环境中定义的变量和函数，而不能直接访问局部环境中的任何数据；
  + 变量的执行环境有助于确定应该何时释放内存。





## 第五章 引用类型
### Object类型
