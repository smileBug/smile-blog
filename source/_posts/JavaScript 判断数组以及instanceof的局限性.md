---
title: JavaScript 判断数组以及instanceof的局限性
date: 2016-04-23 14:13:52
categories:
	JS
tags:
	- instanceof
	- isArray
---
### 检测数组
说到JavaScript里的数组检测，你也许会马上想到用instanceof判断。的确对于一个网页，或者一个全局作用域而言，使用instanceof操作符 就能得到满意的结果：
```javascript
var arr = [1,2,3];
console.log(arr instanceof Array) //true
```
<!-- more -->
与它类似的还有通过原型链判断数组：
```javascript
var arr = [1,2,3];
console.log(arr.constructor == Array) //true
```
用这两种方法来判断数组感觉很美好，但实际上是有漏洞的。
### 局限性
instanceof操作符与constructor属性的问题在于，它们都假定单一的全局执行环境。如果网页中多个框架，那实际上就存在两个以上不同的全局执行环境，从而就存在两个以上版本的Array构造函数。如果arr是另个框架定义的数组，那么上述两种方法就都会返回flase。
### 手动判断
在任何值上调用Object原生的toString()方法，都会返回一个[object NativeConstructorName]格式的字符串。由于原生数组的构造函数名与全局作用域无关，因此使用toString()就能保证返回一致的值。通过Object.prototype.toString()和call()取得任何值的内置属性，把类型检测转换成字符串比较，最终达到检测目的。
```javascript
function isArray(value){    
 return Object.prototype.toString.call(value) == '[object Array]';   
}
var arr = [1,2,3];
console.log(isArray(arr)); //true
```
基于该思路同样也可以检测某个值是不是原生函数或正则表达式：
```javascript
function isFunction(value){    
 return Object.prototype.toString.call(value) == '[object Function]';
}
function isRegExp(value){    
 return Object.prototype.toString.call(value) == '[object RegExp]';
}
```
### 自动判断
此外ECMAScript5新增了Array.isArray()方法。这个方法能自动判断某个值到底是不是数组，而不管它是在哪个全局执行环境中创建的。IE9+、 Firefox 4+、Safari 5+、Opera10.5+和Chrome都实现了这个方法。但是在IE8之前的版本是不支持的。
### 资料参考
1.JavaScript高级程序设计（第3版）
2.[MDN JavaScript 参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)





