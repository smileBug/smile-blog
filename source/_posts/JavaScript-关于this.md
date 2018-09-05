---
title: JavaScript 关于this
date: 2016-07-23 15:15:29
categories:
	JS
tags:
	- this
---
## 关于this
为什么要用this？当然是因为使用它更方便，更优雅啊。不然谁吃饱了撑着花这么多时间去理解这么复杂的玩意！this提供了一种更优雅的方式来隐式“传递”一个对象引用，因此可以将API设计的更加简洁并且易于复用。
<!-- more -->
学习this首先要明白this既不指向函数自身也不指向函数的词法作用域。而且this的绑定实际上是在函数被调用时发生的，它的指向完全取决于函数在哪里被调用。
## 绑定规则
如何判断一个运行中函数的this绑定，就需要先找到这个函数的具体调用位置，然后根据绑定规则来判断this的绑定对象。绑定规则有默认绑定、隐式绑定、显示绑定和new绑定。

### 默认绑定
纯粹的函数调用，直接使用不带任何修饰的函数引用进行调用。此时this就指向全局对象。
例如：
```javascript
function foo(){
    console.log(this.a);
}
var a = 2;
foo();  // 2
```
函数foo()调用时应用了this的默认绑定，因此this绑定的全局对象。
> 注意： 如果使用严格模式，则不能将全局对象用于默认绑定，因此this会绑定到undefined。

### 隐式绑定
当函数作为某个对象的方法被调用时，此时this被绑定到这个上下文对象上。
例如：
```javascript
function foo(){
    console.log(this.a);
}
var obj = {
    a: 2,
    foo: foo
};
obj.foo();  // 2
```
当foo()被调用时，它前面加上了对obj的引用。即实际上是调用了obj对象的foo()方法。此时this被绑定到obj，因此this.a和obj.a是一样的。

如果被隐式绑定的函数丢失了绑定对象，就会应用默认绑定，从而把this绑定到全局对象或者undefin上，取决于是否为严格模式。
例如上述例子：
```javascript
var bar = obj.foo;
var a = "global";
bar();  // "global"
```
这里的bar实际上引用的是foo函数本身，所以此时bar()其实是不带任何修饰的函数调用，因此应用了默认绑定。同理回调函数也是会丢失this绑定。
### 显式绑定
在隐式绑定中，我们必须设置一个对象的内部方法指向所要执行的函数，并通过这个方法间接引用该函数，从而把this隐式绑定到这个对象上。
而在显示绑定中，我们只需要通过apply()和call()方法直接在某个对象上强制调用所要执行的函数。
这两个方法的第一个参数是一个对象(参数为空时，默认调用全局对象)，是为this准备的，接着在调用函数时将其绑定给this。由于可以直接制定this的绑定对象，故称之为显式绑定。
例如：
```javascript
function foo(){
    console.log(this.a);
}
var obj = {
    a: 2
};
foo.call(obj);  // 2
```
通过foo.call(...),在调用foo函数时强制把它的this绑定到obj上。

### new绑定
使用new来调用函数，即构造函数调用，创建一个新对象，此时，this就指向这个新对象。
例如：
```javascript
function foo(a){
    this.a = a;
}
var bar = new foo(2);
console.log(bar.a);  // 2
```
### 判断this绑定对象
综上所述，如果要判断一个运行中函数的this绑定，就需要找到这个函数的直接调用位置。找到后按顺序通过下面四条规则来判定this的绑定对象。

 1. 是否由new调用？绑定到新创建的对象上。
 2. 是否由apply或者call调用？绑定到指定对象是。
 3. 是否作为上下文对象方法调用？绑定到那个上下文对象。
 4. 默认：在严格模式下绑定undefined，否则绑定的全局对象。

## 箭头函数
ES6中介绍了一种无法使用以上规则的特殊函数类型：箭头函数。
箭头函数并不是用function关键字定义的，而是使用操作符“=>”定义的，其this指向根据当前词法作用域来决定的。
例如：
```javascript
function foo(){
    return (a) => {
        console.log(this.a);
    };
}
var obj1 = {
    a: 2
};
var obj2 = {
    a: 3
};
var bar = foo.call(obj1);
bar.call(obj2);  // 2
```
foo()内部创建的箭头函数会捕获调用foo()时的this，由于foo()的this绑定到obj1，所以箭头函数的this也会绑定到obj1。此外，箭头函数的绑定无法被修改。(new也不行)

## 资料参考
1.你不知道的JavaScript（上卷）
