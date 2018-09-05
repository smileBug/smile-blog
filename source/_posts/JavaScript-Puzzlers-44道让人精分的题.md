---
title: postJavaScript Puzzlers 44道令人精分题
date: 2016-09-13 20:37:52
categories:
	JS
tags:
	- 易错
	- 变态
	- 44道题
---
最近做了一个一个“44道JavaScript变态题”。都是比较容易答错的点。
<!-- more -->
原文地址：[javascript-puzzlers](http://javascript-puzzlers.herokuapp.com/)

真的好变态！做完一遍下来你一定会怀疑人生！相信我！

下面是各题总结：
## 第1~10题
### 第1题
```
["1", "2", "3"].map(parseInt)
```
是不是觉得是[1,2,3]。不！你错了！答案是[1,NaN,NaN]。

首先map接受两个参数：每一项运行的回调函数callback，该回调函数的this值。

其中回调函数接受三个参数：数组项的值，该项在数组中的索引，数组对象本身。

题中map只传入回调函数parseInt。

而parseInt只接受两个参数：string，radix（基数,或称进制，即表示把string看成多少进制的值）。radix的合法区间是2-36，当不设置radix的值或者设置为0时，默认为10.
所以本题即为：
```
parseInt('1', 0);
parseInt('2', 1);
parseInt('3', 2);
```
第一个为0被认为是10，而后两个参数不合法。所以答案为[1,NaN,NaN]。

### 第2题
```
[typeof null, null instanceof Object]
```
答案为["object", false]。

这个没啥好说的 typeof null === "object" 这个js就是这么规定。

而null属于原始数据类型，并没有原型对象.
### 第3题
```
[ [3,2,1].reduce(Math.pow), [].reduce(Math.pow) ]
```
答案为 an error

这里考的知识点是数组的reduce方法。
数组的归并方法reduce接受两个参数：一个在每项都调用的回调函数和归并基础的初始值（可选）。

其中回调函数接受四个参数：前一个值、当前值、项的索引和数组对象。

所以第一项等同于 Math.pow(3, 2) ==> 9；Math.pow(9, 1) => 9。

而在没有提供初始值的空数组上执行reduce方法会报TypeError错误
### 第4题
```
var val = 'smtg';
console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');
```
你以为答案会是“Value is Something”？天真！答案是“Something”。

这里考的知识点是优先级。

“+”操作符的优先级大于三元操作符，所以上面就变成了
```
('Value is ' + (val === 'smtg')) ? 'Something' : 'Nothing'
```

非空字符串对应布尔值为true，所以只输出“Something”！
### 第5题
```
var name = 'World!';
(function () {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
```
答案为“Goodbye Jack”

这里考的知识点是声明提升。

var声明提升，而初始化不会提升。
所以就相当于：
```
var name = 'World!';
(function () {
    var name;
    if (typeof name === 'undefined') {
        name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
})();
```
### 第6题
```
var END = Math.pow(2, 53);
var START = END - 100;
var count = 0;
for (var i = START; i <= END; i++) {
    count++;
}
console.log(count);

```
做这题时，我也是想当然地认为答案是101；而正确答案是程序无限循环。

这里考的知识点是js的数字精度。

JavaScript提供的有效数字的精度为53个二进制位，也就是说，绝对值小于2的53次方的整数，即-(2^53-1)到2^53-1，都可以精确表示。

题中Math.pow(2, 53)表示最大值，最大值加一还是最大值，所以循环不会停，自然也不会输出count的值
### 第7题
```
var ary = [0,1,2];
ary[10] = 10;
ary.filter(function(x) {
    return x === undefined;
});
```
答案为 [ ]，而不是[undefined * 7]

这里考的知识点是数组的filter方法。

filter() 方法使用指定的函数测试所有元素，并创建一个包含所有通过测试的元素的新数组。它接受两个参数：一个在每项都调用的回调函数和执行callback时用于this的值（可选）。

callback 只会在已经赋值的索引上被调用，对于那些已经被删除或者从未被赋值的索引不会被调用。那些没有通过 callback 测试的元素会被跳过，不会被包含在新数组中。
### 第8题
```
var two   = 0.2
var one   = 0.1
var eight = 0.8
var six   = 0.6
[two - one == one, eight - six == two]
```
答案是 [true , false]。

考的知识点是 数字精度计算。

至于什么时候精确什么时候不精确？I don't know.

### 第9题
```
function showCase(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase(new String('A'));
```
答案是 'Do not know!'

new String('A')返回的是String实例，是对象。和字符串'A'不相等。
### 第10题
```
function showCase2(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase2(String('A'));
```
答案是 'Case A'

和上题一样只不过直接调用String()返回的是字符串。
## 第11~20题
### 第11题
```
function isOdd(num) {
    return num % 2 == 1;
}
function isEven(num) {
    return num % 2 == 0;
}
function isSane(num) {
    return isEven(num) || isOdd(num);
}
var values = [7, 4, '13', -9, Infinity];
values.map(isSane);
```
答案是 [true, true, true, false, false]

前两个没问题，'13'会转换成number类型再运算，余数的正负号随第一个操作数。所以-9 % 2 => -1。而 Infinity % 2 => NaN。
### 第12题
```
parseInt(3, 8)
parseInt(3, 2)
parseInt(3, 0)
```
答案是 3，NaN，3

参考第一题
### 第13题
```
Array.isArray( Array.prototype )
```
答案是 true

Array.prototype => [];
### 第14题
```
var a = [0];
if ([0]) {
  console.log(a == true);
} else {
  console.log("wut");
}
```
答案是 false

在if( )判断语句里作为单独的布尔值，是true；

而如果进行比较的话，就是false。

### 第15题
```
[]==[]
```
答案是 false

数组是引用类型，就算是表面相等的两个数组也不是同一个数组。
### 第16题
```
'5' + 3
'5' - 3
```
答案是 '53', 2

这个很简单，“+” 用来表示两个数的和或者字符串拼接， “-”表示两数之差。
### 第17题
```
1 + - + + + - + 1
```
答案是 2

我觉得只要看两数字间“-”的个数，负负得正。
### 第18题
```
var ary = Array(3);
ary[0]=2
ary.map(function(elem) { return '1'; });
```
答案是 ["1", undefined × 2]

和第7题类似，map方法和filter方法一样会跳过未被赋值的项。
### 第19题
```
function sidEffecting(ary) {
  ary[0] = ary[2];
}
function bar(a,b,c) {
  c = 10
  sidEffecting(arguments);
  return a + b + c;
}
bar(1,1,1)
```
答案是 21

在非严格模式下，修改arguments或修改参数时会互相影响。而在严格模式下arguments是静态副本，和参数不共享相同的值
### 第20题
```
var a = 111111111111111110000,
    b = 1111;
a + b;
```
答案是 111111111111111110000

大概是111111111111111110000大于js的最大值了，所以加不上？嗯 应该是。
## 第21~30题
### 第21题
```
var x = [].reverse;
x();
```
答案是 window

它给的解释是

> [ ].reverse will return this and when invoked without an explicit receiver object it will default to the default this AKA window

也就是说，当没有明确传入this值时，会返回这个调用者。这里在全局调用，所以返回window。
但我在各个浏览器试了，都没有返回window，而是报错说Array.prototype.reverse 的‘this’ 为空或未定义。
### 第22题
```
Number.MIN_VALUE > 0
```
答案是 true

这里有个坑，Number.MIN_VALUE是大于零最小的数，而不是最小的负数，那个是-Number.MAX_VALUE。

### 第23题
```
[1 < 2 < 3, 3 < 2 < 1]
```
答案是 [true, true]

所谓的 "1 < 2 < 3" 其实先比较 "1 < 2" 为true，再比较" true < 3",这里true被转换成数字1， 1 < 3 ,所以为true，第二项类似。
### 第24题
```
// the most classic wtf
2 == [[[2]]]
```
答案是 true

我估计是不管嵌套多少层数组，js都会把它转换成包含的第一个数字，例如 [[[21,2]]] => 21 ; [[[[[1],[2],[3]]]]] => 1.
### 第25题
```
3.toString()
3..toString()
3...toString()
```
答案是 **error  “3”  error**

首先“3.toString()”尽管对于number类型调用方法时会先进行包装，但是这里的点是小数点还是方法调用的点？js会把它认为是小数点，所以结果是“error”

第二个“3..” 第一个点被解释为小数点，变成(3.).toString(),所以结果是“3”

第三个中，第一个点是小数点，第二个点是方法调用的点，但是再后面接个点就成了非法的方法名，于是是“error”
### 第26题
```
(function(){
  var x = y = 1;
})();
console.log(y);
console.log(x);
```
答案是 **1， error**

原式换个形式：
```
(function(){
  var x;
  x = 1;
  y = 1;
})();
```
x为局部变量，在全局环境中无法被访问到，而y被隐式地定义成了全局变量，所以能在全局环境中访问。
### 第27题
```
var a = /123/,
    b = /123/;
a == b
a === b
```
答案是 **false， false**

JavaScript中的正则表达式是对象，所以尽管a和b看着一样，但是他俩不是同一个对象，不管 “==” 还是 “===” 都是false。
### 第28题
```
var a = [1, 2, 3],
    b = [1, 2, 3],
    c = [1, 2, 4]
a ==  b
a === b
a > c
a < c
```
答案是 **false,false,false,true**

首先与上题类似，数组为对象，所以前两个为false

而js里的数组比较大小，类似字符串比较，会从第一个元素开始比较，一个一个比。

### 第29题
```
var a = {}, b = Object.prototype;
[a.prototype === b, Object.getPrototypeOf(a) === b]
```
答案是 **false， true**

原型链的题。主要要搞清除__proto__和prototype的区别。实例对象的__proto__会指向其构造函数的prototype属性（即原型对象）。

这里Object为构造函数，所以b其实为Object的原型对象，使用字面量创建对象和new Object创建对象是一样的，所以a.__proto__也就是Object.prototype。而Object.getPrototypeOf(a)方法是获得a的原型对象，与a.__proto__效果一样。所以第二个为true。

而实例对象a并没有prototype属性，所以a.prototype其实为undefined。

### 第30题
```
function f() {}
var a = f.prototype, b = Object.getPrototypeOf(f);
a === b
```
答案是 false

参考上题
## 第31~40题
### 第31题
```
function foo() { }
var oldName = foo.name;
foo.name = "bar";
[oldName, foo.name]
```
答案是 ** “foo”， “foo”**

function对象本书的name的属性是只读的不允许更改。
### 第32题
```
"1 2 3".replace(/\d/g, parseInt)
```
答案是 **“1 NaN 3”**

与第一题类似，parseInt()传入两个参数：该项的值与该项所在的索引。
### 第33题
```
function f() {}
var parent = Object.getPrototypeOf(f);
f.name // ?
parent.name // ?
typeof eval(f.name) // ?
typeof eval(parent.name) //  ?
```
答案是 **"f", "Empty", "function", undefined**

f.name值为"f"，而eval("f")则会输出f函数，所以结果为"function".

而parent实际上就是f.__proto__,而函数也是对象，它的原型就是一个名为Empty（空）的function。在全局作用域下调用Empty，显示未定义。
### 第34题
```
var lowerCaseOnly =  /^[a-z]+$/;
[lowerCaseOnly.test(null), lowerCaseOnly.test()]
```
答案是 **true， true**

这里 test 函数会将参数转为字符串. “null”, “undefined” 自然都是全小写。
### 第35题
```
[,,,].join(", ")
```
答案是 **“, , ”**

js在使用字面量创建数组时，如果末尾有个逗号，会被忽略，所以该数组长度为3，并且时稀疏数组（[undefined × 3]），而对三个元素的数组使用join方法，只需添加两个逗号。
### 第36题
```
var a = {class: "Animal", name: 'Fido'};
a.class
```
答案是 **不确定**

class是保留字，不同浏览器处理方式不同。
### 第37题
```
var a = new Date("epoch")
```
答案是 **“Invalid Date”**

调用 Date 的构造函数传入一个字符串的话需要符合规范, 即满足 Date.parse 的条件.如果格式错误 构造函数返回的仍是一个Date的实例 Invalid Date.
### 第38题
```
var a = Function.length,
    b = new Function().length
a === b
```
答案是 **false**

首先函数原型对象Function.length定义为1，其次function(Function 的实例)的 length 属性就是函数签名的参数个数, 所以 b.length == 0
### 第39题
```
var a = Date(0);
var b = new Date(0);
var c = new Date();
[a === b, b === c, a === c]
```
答案是 **false，false，false**

关于Date类型：
1.如果不传参数等价于当前时间
2.如果是函数调用返回一个字符串

所以a为当前时间的字符串，b为1970年的那个初始时间，而c为当前时间。


### 第40题
```
var min = Math.min(), max = Math.max()
min < max
```
答案是 **false**

又是一道“不正常”的题

Math.min不传参数返回“Infinity”，而Math.max不传参数返回“-Infinity”。
## 第41~44题
### 第41题
```
function captureOne(re, str) {
  var match = re.exec(str);
  return match && match[1];
}
var numRe  = /num=(\d+)/ig,
    wordRe = /word=(\w+)/i,
    a1 = captureOne(numRe,  "num=1"),
    a2 = captureOne(wordRe, "word=1"),
    a3 = captureOne(numRe,  "NUM=2"),
    a4 = captureOne(wordRe,  "WORD=2");
[a1 === a2, a3 === a4]
```
答案是 **[true, false]**

第一个正则有一个 g 选项 它会‘记忆’他所匹配的内容, 等匹配后他会从上次匹配的索引继续, 即使它们是在不同字符串中使用而第二个正则不会.

所以a1 = a2 = “1”; 而a3 = “null”， a4 = “2”。
### 第42题
```
var a = new Date("2014-03-19"),
    b = new Date(2014, 03, 19);
[a.getDay() === b.getDay(), a.getMonth() === b.getMonth()]
```
答案是 ** [false, false]**

不懂。
```
a.getDay() //3
b.getDay() //6
a.getMonth() //2
b.getMonth() //3
```
### 第43题
```
if ('http://giftwrapped.com/picture.jpg'.match('.gif')) {
  'a gif file'
} else {
  'not a gif file'
}
```
答案是 **“a gif file”**

正则表达式中的点号没有被转义导致其被解释成匹配除换行符以外的任意字符。
### 第44题
```
function foo(a) {
    var a;
    return a;
}
function bar(a) {
    var a = 'bye';
    return a;
}
[foo('hello'), bar('hello')]
```
答案是 **[“hello”， “bye”]**

两个函数中，a作为参数其实已经被声明了。
## 总结
真的好难，如果是第一次做，真的有好多坑。

回头总结，发现好多其实是基础知识，只是平时没注意，理解不够深。

还是得好好看基础。。。。
