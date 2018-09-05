---
title: ES6学习笔记之let和const
date: 2016-09-06 10:15:55
categories:
	ES6
tags:
	- let
	- const
	- 块级作用域
---
## 块级作用域
JavaScript的ES5规范中只有全局作用域和函数作用域，这带来很多不合理的地方。比如内层变量覆盖外层变量,用来计数的循环变量泄露为全局变量。ES6中为我们提供了let和const命令来构造块级作用域。
<!-- more -->
## let命令
ES6新增了let命令来申明变量，其用法类似var。不过let声明的变量，只在所在的代码块内有效。也就是形成了JavaScript所没有的**块级作用域**。
### 基本用法
直接来例子：
```
{
    let a = 2;
    var b = 3;
}
console.log(a);   //ReferenceError: a is not defined.
console.log(b);   //3
```
在代码块中，分别用let和var声明了两个变量，然后在代码块外调用了这两个变量，结果var声明的变量返回了正确的值，而let声明的变量报错。这表明了let声明的变量只在它所在的代码块中有效 。

let声明在循环中极其好用，之前在闭包中有提到：[JavaScript 闭包](http://smilebug.me/2016/07/26/JavaScript-%E9%97%AD%E5%8C%85/);
### 不存在变量提升
总所周知，var声明变量和函数声明都会提前，而let声明不会，变量在声明前使用就会报错。
例如：
```
console.log(a); ////ReferenceError: a is not defined.
console.log(b); //undefined
let a = 2;
var b = 3;
```
### 暂时性死区
如果区块中存在let命令，这个区块对以let声明的变量，从一开始就行形成了封闭作用域。凡是在声明前就使用这些变量，就会报错。（总之，在代码块内，使用let声明变量前，该变量都是不可用的，即使在全局作用域中有这个对象）
例如：
```
var a = 2;
{
    console.log(a); //ReferenceError: a is not defined
    let a = 3;
}
```
### 不允许重复声明
let不允许在相同作用域内，重复声明同一个变量。因此也不能在函数内部重新声明参数。
## const命令
ES6中的const命令声明一个只读的常量，一旦声明，常量的值就不能改变，这也意味着，const一旦声明变量就必须立即赋值，而不能等到以后赋值。
### const使用注意
const的作用域和let相同：只在声明的块级作用域中有效。

const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

const声明的常量，也与let一样不可重复声明。
### 复合类型的变量声明
对于复合类型的变量，变量名不指向数据，而指向数据所在的地址。const命令只是保证变量名指向的地址不变，而不保证该地址的数据不变。
## 全局对象的属性
全局对象是最顶层的对象，在浏览器环境指的是window对象，在Node.js指的是global对象。ES5之中，全局对象的属性与全局变量是等价的。
ES6为了改变这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是全局对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于全局对象的属性。也就是说，从ES6开始，全局变量将逐步与全局对象的属性脱钩。
```
var a = 1;
console.log(a);  //1
let b = 2;
consle.log(b)  //undefined
```
上面代码中，全局变量a由var命令声明，所以它是全局对象的属性；全局变量b由let命令声明，所以它不是全局对象的属性，返回undefined。
## 资料参考
[《ES6 标准入门》](http://es6.ruanyifeng.com/#docs/let)
