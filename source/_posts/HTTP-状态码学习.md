---
title: HTTP 状态码学习（喵星人版）
date: 2016-08-07 13:31:45
categories:
    HTTP
tags:
	- 状态码
	- 喵星人
---
## HTTP学习
个人觉得作为一个有理想有抱负的前端工程师HTTP这块的知识是非常有必要掌握的，熟悉了HTTP协议，以后进行调试的时候，才能看明白捕获回来的HTTP请求，并且知道是不是其中某个地方有问题导致出现你想要修复的bug，如何处理能够最终修复这个bug。
<!-- more -->
## HTTP状态码
状态码的职责是当客户端向服务器端发送请求时，描述返回的请求结果。借助状态码，用户可以知道服务器端是正常处理了请求，还是出现了错误。

状态码由3位数字和原因短语组成，数字中第一位指定相应类别，后两位无分类，有以下5中响应类别。

| 状态码     | 类别   												 | 原因短语  										|
| --------   | :----- 												 | :----  											|
| 1XX        | Informational(信息性状态码)		 | 接受的请求正在处理   			  |
| 2XX        | Success(成功状态码)  					 | 请求正常处理完毕  						|
| 3XX        | Redirection(重定向状态码)   		 | 需要进行附加操作以完成请求   |
| 4XX        | Client Error(客户端错误状态码)  | 服务器无法处理请求  					|
| 5XX        | Server Error(服务器错误状态码)  | 服务器处理请求出错  					|


网上看到HTTP趣味记法。通过ps了一系列的HTTP信息，让人印象深刻。[原文地址](http://rightyaleft.com/funhumor/http-status-codes-with-meooowwwwww-using-cat-photos/)
原文状态码比较全，这里我只选取了部分比较正常常用的。
## 2XX 成功
2XX的响应结果表明请求被正常处理了。
### 200 OK
![](http://7xsb4y.com1.z0.glb.clouddn.com/200.jpg)
最常见的“200 OK”表示从客户端发来的请求在服务器端被正常处理了。
### 204 No Content（无内容）
![](http://7xsb4y.com1.z0.glb.clouddn.com/204.jpg)
该状态码代表请求已成功被服务器处理，但没有返回任何实体内容。

例如浏览器发送请求处理后，返回204响应，浏览器显示页面就不发生更新。

一般只需要从客户端往服务器发送信息，而对客户端不需要发送新信息内容的情况下使用。
### 206 Partial Content（局部内容）
该状态码表示客户端进行请求头中包含Range字段指定范围的请求，服务器端成功执行GET请求，响应报文中包含Content-Range指定范围的实体内容。
## 3XX 重定向
3XX响应结果表明浏览器需要执行某些特殊的处理来正确处理请求。
### 301 Moved Permanently（永久重定向）
![](http://7xsb4y.com1.z0.glb.clouddn.com/301.jpg)
永久重定向。

该状态码表示请求的资源被分配了新的URI，以后应使用响应报文头部中Location所指的URI。

举个例子，如果已经把资源对应的URI保存为书签，这时应该按Location首部字段提示的URI重新保存。
### 302 Found（找到）
![](http://7xsb4y.com1.z0.glb.clouddn.com/302.jpg)
临时重定向。

该状态码表示请求的资源被分配了新的URI，本次应使用响应报文头部中Location所指的URI。

举个例子，如果已经把资源对应的URI保存为书签，不需要重新保存。
### 304 Not Modified（未修改）
![](http://7xsb4y.com1.z0.glb.clouddn.com/304.jpg)
该状态码表示资源自上次请求以来没有被改变。

304状态码返回时，不包含任何响应的主体部分。
## 4XX 客户端错误
4XX的响应结果表明客户端是发生错误的原因所在。
### 400 Bad Request（错误请求）
![](http://7xsb4y.com1.z0.glb.clouddn.com/400.jpg)
该状态码表示请求报文中存在语法错误。当错误发生时，需要修改请求的内容后再次发送请求。
### 401 Unanthorized（未授权）
![](http://7xsb4y.com1.z0.glb.clouddn.com/401.jpg)
该状态码表示发送的请求需要有通过HTTP认证(BASIC认证、DIGEST认证)的认证信息。

返回401的响应必须包含一个适用于被请求资源的WWW-Authenticate首部用以质询(challenge)用户信息。

浏览器初次接受401响应，会弹出认证用的对话窗口。输入密码、用户名之后会再次请求，此时的请求头中会包含Authorization首部字段，用来说明认证算法、用户和密码。如果授权证书是正确的，服务器就会将文档返回。
### 403 Forbidden（禁止）
![](http://7xsb4y.com1.z0.glb.clouddn.com/403.jpg)
该状态码表明对请求资源的访问被服务器拒绝了。
### 404 Not Found（未找到）
![](http://7xsb4y.com1.z0.glb.clouddn.com/404.jpg)
该状态码表示服务器上无法找到请求的资源。除此之外，也可以在服务器拒绝请求且不想说明的理由时使用。
## 5XX 服务器错误
5XX的响应结果表明服务器本身发生错误。
### 500 Internal Server Error（服务器内部错误）
![](http://7xsb4y.com1.z0.glb.clouddn.com/500.jpg)
该状态码表明服务器在执行请求时发生了错误。也有可能是Web应用存在的bug活某些临时故障。
### 503 Service Unavailable（服务不可用）
![](http://7xsb4y.com1.z0.glb.clouddn.com/503.jpg)
该状态码表明服务器暂时处于超负荷或正在进行停机维护，现在无法处理请求。如果事先得知解除以上状况需要的时间，最好写入Retry-After首部字段再返回给客户端，该字段会告知客户端应该在多久之后再次发送请求。
## 参考资料
1.《图解HTTP》
2.[喵星人教你HTTP状态码](http://rightyaleft.com/funhumor/http-status-codes-with-meooowwwwww-using-cat-photos/)
