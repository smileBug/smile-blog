---
title: ES6学习笔记之Promise
date: 2016-08-18 16:47:59
categories:
	ES6
tags:
	Promise
---
### 概念
Promise是JavaScript针对异步操作场景的解决方案

Promise首先是个对象，用来传递异步操作的消息。它代表了某个未来才会知道结果的事件，并且这个事件提供统一的API，可供进一步处理。
<!-- more -->
以同步方式来写代码，但是执行方式是异步的，但是保证程序的执行顺序是同步的。
### 三种状态
Promise只有三种状态：未完成（pending）、已完成（fulfilled 或 Resolved）和失败（rejected）
这三种状态的变化可以从未完成-->已完成，或者从未完成-->失败。
过程只能发生一次，并且是不可逆的，也就是说不能从未完成-->已完成-->失败，也不能从已完成或者失败-->未完成
### 特点
Promise的链式写法，程序流程清晰。更易于维护。
### 实现例子
通过Promise实现的一个小动画
[在线演示](http://smilebug.me/demos/ball.html)
```
//获取三个球
var ball1 = document.querySelector('.ball1');
var ball2 = document.querySelector('.ball2');
var ball3 = document.querySelector('.ball3');
// Promise实现
function promiseAnimate(ball,distance){
	return new Promise(function(resolve,reject){
		function _animate(){
			setTimeout(function(){
				var marginLeft = parseInt(ball.style.marginLeft);
				if (marginLeft == distance) {
					resolve()
				} else {
					if (marginLeft < distance){
						marginLeft++;
					} else {
						marginLeft--;
					}
					ball.style.marginLeft = marginLeft + 'px';
					_animate();
				}
			},13)
		}
		_animate();
	})
}

promiseAnimate(ball1,100)
	.then(function(){
		return promiseAnimate(ball2,100);
}).then(function(){
		return promiseAnimate(ball3,100);
}).then(function(){
		return promiseAnimate(ball1,200);
}).then(function(){
		return promiseAnimate(ball2,200);
}).then(function(){
		return promiseAnimate(ball3,200);
})
```

