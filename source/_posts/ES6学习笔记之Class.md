---
title: ES6学习笔记之Class
date: 2016-09-07 09:58:28
categories:
	ES6
tags:
	- Class
	- extends
	- super
---
## Class
要说ES5中最让人头疼的部分，那原型，构造函数和继承这三个一定排得上前几，复杂难懂的语法，扑朔迷离的指针无不让我们纠结万分。
<!-- more -->
为此ES6提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。
### Class概述
基本上，ES6的class可以看作只是一个语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。
例如：
```
class People{
    constructor(name,age){
        this.name = name;
        this.age = age;
    }

    say(){
        console.log('I am ' + this.name)
    }
}
```
上面代码首先用class定义了一个“类”，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。这里的构造方法对应的就是ES5中的构造函数。
简单地说，constructor内定义的方法和属性是实例对象自己的，而constructor外定义的方法和属性则是所有实例对象可以共享的。
ES6的类，完全可以看作构造函数的另一种写法。类的数据类型就是函数，类本身就指向构造函数。
例如：
```
class People{
    //...
}

typeof People;   //"function"
People === People.prototype.constructor;   //true
```

### constructor方法
constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。

### 类的实例对象
生成类的实例对象的写法，与ES5完全一样，也是使用new命令。但如果忘记加上new，像函数那样调用Class，将会报错。而普通构造函数不用new也可以执行。这是两者的主要区别。
```
var p1 = new People('Tony',22); //正确
var p2 = People('Lucy',23);  //报错
```
### 不存在变量提升
ES6不会把类的声明提升到代码头部。也就是在定义类前使用它就会报错。这种规定的原因与继承有关，必须保证子类在父类之后定义。

## Class的继承
### extends关键字
Class之间可以通过extends关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。
```
class Actor extends People {}
```
上面代码定义了一个Actor类，该类通过extends关键字，继承了People类的所有属性和方法。但是由于没有部署任何代码，所以这两个类完全一样，等于复制了一个People类。下面在Actor内部加上代码 。
```
class Actor extends People {
    constructor(name,age,habbit){
        super(name,age);
        this.habbit = habbit;
    }

    say(){
        console.log('I am ' + this.name + ', and I like ' + this.habbit)
    }
}
```
上面代码中，constructor方法和say方法之中，都出现了super关键字，它在这里表示父类的构造函数，用来新建父类的this对象。
子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。
### super关键字
super这个关键字，有两种用法，含义不同。

（1）作为函数调用时（即super(...args)），super代表父类的构造函数。

（2）作为对象调用时（即super.prop或super.method()），super代表父类。注意，此时super即可以引用父类实例的属性和方法，也可以引用父类的静态方法。
## 资料参考
[《ES6 标准入门》](http://es6.ruanyifeng.com/#docs/let)
