---
title: MooTools 源码分析之Core (未完待续)
date: 2016-08-02 17:04:55
categories:
	MooTools
tags:
	- 源码分析
---888
```javascript
(function(){
	//代码被包在一个匿名函数的内部，所以这里的this明确指向全局对象。
	//版本信息
this.MooTools = {
	version: '1.6.0',
	build: '529422872adfff401b901b8b6c7ca5114ee95e2b'
};
```
<!-- more -->
```
// 定义了typeOf, instanceOf两个函数来替代JS原生的typeof和instanceof方法(注意大小写)

//定义typeOf方法(注意O大写！),在原生typeof的基础上做了拓展,判断参数类型
var typeOf = this.typeOf = function(item){
	//如果对象是null则返回null
	if (item == null) return 'null';
	if (item.$family != null) return item.$family();

	//判断node类型
	if (item.nodeName){
		//如果nodeType的值为1，则返回element
		if (item.nodeType == 1) return 'element';
		//如果nodeType的值为3，判断其有无值，选择返回值或者空白格
		if (item.nodeType == 3) return (/\S/).test(item.nodeValue) ? 'textnode' : 'whitespace';
	} else if (typeof item.length == 'number'){
		if ('callee' in item) return 'arguments';				//如果有callee属性的则返回arguments
		if ('item' in item) return 'collection';				//如果有item属性的则返回collection
	}
	//如果都不满足则返回原生JS typeof运算符的结果："undefined","boolean","string","number","object","function"
	return typeof item;
};
```
```
//定义instanceOf方法(注意O大写！)，在原生instanceof的基础上做了拓展,判断某个对象是否是某个特定类型的实例
var instanceOf = this.instanceOf = function(item, object){
	//如果对象为null则返回false
	if (item == null) return false;
	//定义constructor属性 判断是否可以追溯到object，如果可以则返回true
	var constructor = item.$constructor || item.constructor;
	while (constructor){
		if (constructor === object) return true;
		constructor = constructor.parent;
	}
	/*<ltIE8>*/	//------------>前后两个/*<ltIE8>*/表示为了兼容IE8.。。。。。。万恶的IE！！！
	if (!item.hasOwnProperty) return false;
	/*</ltIE8>*/
	//如果都不满足则返回原生JS typeof运算符的结果
	return item instanceof object;
};
```
```
var hasOwnProperty = Object.prototype.hasOwnProperty;

/*<ltIE8>*/

var enumerables = true;
//对IE中不能遍历对象中toString属性的bug做一个修正
for (var i in {toString: 1}) enumerables = null;
//如果遍历不到则enumerables为true，然后将这些属性暂存到一个数组保存到enumerables中
if (enumerables) enumerables = ['hasOwnProperty', 'valueOf', 'isPrototypeOf', 'propertyIsEnumerable', 'toLocaleString', 'toString', 'constructor'];

//该函数就是传进object和bind两个对象和fn方法，首先遍历判断object对象里是否有enumberables数组中的那些属性，
//如果有则让bind对象执行fn方法，该方法传入k和object[k]两个参数
function forEachObjectEnumberableKey(object, fn, bind){
	if (enumerables) for (var i = enumerables.length; i--;){
		var k = enumerables[i];
		//hasOwnProperty.call(object, k) --> object.hasOwnProperty(k) -->object对象是否有自己的属性k
		//fn.call(bind, k, object[k]) --> bind.fn(k,object[k]) -->bind对象执行fn方法并传入参数k和object[k]
		if (hasOwnProperty.call(object, k)) fn.call(bind, k, object[k]);
	}
}
/*</ltIE8>*/

```
