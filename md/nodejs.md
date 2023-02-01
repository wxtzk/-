[TOC]
# 👽模块系统
[`node官方文档`](http://nodejs.cn/api/)

> Node.js 有两个模块系统：CommonJS 模块和 ECMAScript 模块
## ECMAScript标准的缺陷
1. 没有模块系统
2. 标准库较少
3. 没有标准接口
4. 缺乏管理系统
## 现实要求
* 如果程序设计的规模达到一定程度，则必须对其进行模块化
* 模块可以有多种形式，但至少应该提供能够将代码分割为多个源文件的机制
* CommonJS的模块功能可以帮助我们解决该问题
## CommonJS
* CommonJS规范的提出主要是弥补当前js没有标准的缺陷
* CommonJS规范希望js代码可以在任何地方运行
* CommonJS对模块的定义非常简单：
	* 模块定义
	* 模块引用
	* 模块标识
### 模块定义
* 在node种，一个js文件就是一个模块
* 在node中，每一个js文件的代码都是独立运行在一个函数中，而不是全局作用域，所以一个模块中的变量和函数在其他模块中无法直接访问
* 通过exports向外暴露变量和方法（exports向外部暴露的是一个对象）
### 模块引用
* 在node中，通过require（）引入外部模块
* 使用require（）引入外部模块后，该函数会返回一个对象，该对象即是exports暴露的对象，也代表这个被引入的模块
> 引用步骤
1. 路径分析
2. 文件定位
3. 编译执行
### 模块标识
* 模块标识就是模块的名字，也就是传递给require()的参数，路径可以是绝对路径和相对路径，相对路径只能以**./**和**../**开头
### 模块分类
* 底层C++编写的**内建模块**
* node提供的**核心模块**
* 用户编写的**文件模块**
### global
* 全局对象global于网页中的window类似
* 在全局中创建的属性会作为global属性
* 在全局中创建的方法会作为global的方法
```javascript
a = 10
b = function (){
	console.log("我是b方法")
}
console.log(global.a)
global.b()
```
### 如何证明js模块独立运行在一个函数中？
> 实际编写代码
```javascript
//argument参数对象,arguments.callee返回当前函数内容
console.log(arguments)
console.log(arguments.callee+"")
```
>实际执行代码
```javascript
function (exports, require, module, __filename, __dirname) {
    //argument参数对象,arguments.callee返回当前函数内容
	console.log(arguments)
	console.log(arguments.callee+"")
}
//在node执行模块前，会首先为模块套上一个函数，该函数有5个参数
//exports：模块向外暴露的对象
//require：引用外部模块函数
//module：模块自身，其中exports是module的一个属性
//__filename：js文件的路径
//__dirname：js文件夹的路径
```
### exports于module.exports的区别与联系
> 联系

`exports = modules.exports = {}`这是再开始执行某模块代码时自动加上的

>区别
* exports只能通过 . 的形式使用
* module.exports既可以通过 . 的形式使用，也可以直接进行对象赋值
* `原因`：exports保存的是指向modues.exports地址值，如果直接进行对象赋值，相当于创建了新的对象，没有起到真正修改module.exports对象的目的, 导致require引用不到exports里的值
## 包（package）
> CommonJS允许将一组相关的模块组合到一起，形成一个完整的工具包
### 包组成
* 包结构：用于组织包中的各种文件
* 包描述文件：描述包的相关信息，以供外部读取
### 包结构
> 其实就是包解压之后的目录结构
```
package.json：描述文件（必须）
bin：可执行二进制文件
lib：js代码
doc：文档
test：测试
```
### 包描述文件
> 用于包的相关信息的文件，即package .json
### NPM(Node Package Manager)
> 包管理系统，CommonJS规范的一种实现

[NPM教程](https://blog.csdn.net/u011342720/article/details/81267908)
[npm的镜像管理工具](https://blog.csdn.net/qq_38872934/article/details/105706101)

### NPM包搜索流程
```
在node中使用模块名字引入模块时，首先在node_modules中找，
如果有直接使用，如果没有则去上一级目录的node_modiles中找，
如果有直接使用，如果没有再向上一级找，直到找到为止，
如果找到根目录仍然没有找到，则报错。
```
# 🐲文件系统
* node通过fs模块来和文件系统进行交互
* fs模块提供了标准文件访问API，可以对文件进行各种操作
* fs模块的所有操作都提供两种方式，**同步和异步**
* 使用fs模块，需要加载，即`requir('fs')`
## Buffer(缓冲区)
- 结构上，Buffer像一个字节数组，因此Buffer元素的取值范围为0-255
- 实质上，Buffer直接通过C++操作内存，不是通过js操作内存
- 操作上，操作方式于操作数组的方式类似，而且不需要引入模块，可以直接使用
`Buffer的大小一旦确定就不能更改，Buffer实际上是对底层内存的修改`
`Buffer的使用过程就是对需要缓存内容的编码与解码，默认utf8格式`
```javascript
var buf = Buffer.from('Hello,world.')
console.log(buf)
//创建一个指定大小的Buffer，不推荐使用new的方式
//var buf1 = new Buffer(10)
//创建一个指定大小的Buffer，推荐使用
var buf1 = Buffer.alloc(20,'Hello,world.')
console.log(buf1)
console.log(buf.toString())
console.log(buf1.toString("utf16le"))
```
## 文件操作举例
| 模式 |                        说明                        |
| :--: | :------------------------------------------------: |
|  r   |              读文件，文件不存在则报错              |
|  r+  |             读写文件，文件不存在则报错             |
|  rs  |            在同步模式下打开文件用于读取            |
| rs+  |            在同步模式下打开文件用于读写            |
|  w   |    打开文件用于写操作，不存在则创建，存在则截断    |
|  wx  |     打开文件用于写操作，如果**存在**则打开失败     |
|  w+  |   打开文件用于读写，如果不存在则创建，存在则截断   |
| wx+  |      打开文件用于读写，如果**存在**则打开失败      |
|  a   |          打开文件用于追加写，不存在则创建          |
|  ax  |   打开文件用于追加写，如果**路径存在**则打开失败   |
|  a+  |      打开文件用于读取和追加，如果不存在则创建      |
| ax+  | 打开文件用于读取和追加，如果**路径存在**则打开失败 |
> 同步写文件
```javascript
var fs = require('fs')
//打开文件(如果文件不存，会自动创建)，
//该方法会返回一个Number类型的文件描述符，我们可以通过该描述符对文件进行各种操作
var fd = fs.openSync('hello.txt','w')
//console.log(fd)
//文件写入
fs.writeSync(fd,'鹤壁市医疗保障局')
//关闭文件
fs.closeSync(fd)
```
> 异步写文件
```javascript
var fs = require('fs')
//异步方法没有返回值，结果数据都是通过回调函数参数接收
//可以通过arguments对象查看参数
//打开文件
fs.open('hello.txt','w',function(err,fd){
	console.log(arguments)
	if(!err){
		//文件写入
		fs.write(fd,'鹤壁市医疗保障局',function(err,written,string){
			console.log(arguments)
		})
		//关闭文件
		fs.close(fd,function(err){
			console.log(arguments)
		})
	}else{
		console.log(err)
	}	
})
```
> 简单文件写入
```javascript
//fs.writeFile(file,data[,options],callback)
//fs.writeFileSync(file,data[,options])
//file:文件路径
//data：要写入的数据
//options：选项，可以对写入进行配置
//callback：回调函数
var fs = require('fs')
//直接写入
fs.writeFile('hello.txt','这是写入的内容******',function(err){
	if(!err){
		console.log('success')
	}
})
//简单文件写入是对前面文件写入过程的一个封装
```
> 流式文件写入

`同步、异步、简单文件写入都不适合大文件写入，性能较差，容易导致内存溢出`
```javascript
var fs = require('fs')
//创建一个可写流。fs.createWriteStream(path[,options])
var ws = fs.createWriteStream('hello.txt')
//可以通过监听open和close事件来监听流的打开和关闭
//once()为对象绑定一个一次性事件，触发一次后自动失效
ws.once('open',function(){
	console.log('流打开了~~~')
})
ws.once('close',function(){
	console.log('流关闭了~~~')
})
//向流里写内容
ws.write('通过可写流写入的内容')
ws.write('鹤壁市医疗保障局')
ws.write('流式文件写入')
ws.write('hello,wrold!')
//关闭流
//ws.close()//关闭流出端口
ws.end()//关闭流入端口
```
> 简单文件读取
```javascript
var fs = require('fs')
//fs.readFile(path[,options],callback)
//fs.readFileSyns(path[,options])
fs.readFile('hello.txt',function(err,data){
	//console.log(arguments)
	if(!err){
		console.log(data.toString())
	}else{
		console.log(err)
	}
})
```
>流式文件读取
```javascript
var fs = require('fs')
//适用于读取较大的文件，可以分多次读取到内存中
//创建一个可读流
var rs = fs.createReadStream('img1.png')
//创建一个可写流
var ws = fs.createWriteStream('imgCopy.png')
//监听流的开启和关闭
rs.once('open',function(){
	console.log('可读流开启了~~~')
})
rs.once('close',function(){
	console.log('可读流关闭了~~~')
})
ws.once('open',function(){
	console.log('可写流开启了~~~')
})
ws.once('close',function(){
	//关闭可写流
	console.log('可写流关闭了~~~')
	//当流中没有更多数据可供消费时，则会触发 'end' 事件。
	//除非数据被完全地消费，否则不会触发 'end' 事件。 
    //这可以通过将流切换到流动模式来实现，或者通过重复调用 stream.read() 直到所有数据都被消费完。
	ws.end()
})
//要读取可读流中的数据，必须绑定一个data事件
//绑定data事件后会自动读取数据，并通过回调函数接收数据
//on()为对象添加监听事件，会对该对象进行持续监听
rs.on('data',function(data){
	//每次最多读取65536个字节
	//console.log(arguments)
	//将读取到的写到可写流
	ws.write(data)
})
```
> 使用管道
```javascript
var fs = require('fs')
//适用于读取较大的文件，可以分多次读取到内存中
//创建一个可读流
var rs = fs.createReadStream('img1.png')
//创建一个可写流
var ws = fs.createWriteStream('imgCopy.png')
//监听流的开启和关闭
rs.once('open',function(){
	console.log('可读流开启了~~~')
})
rs.once('close',function(){
	console.log('可读流关闭了~~~')
})
ws.once('open',function(){
	console.log('可写流开启了~~~')
})
ws.once('close',function(){
	console.log('可写流关闭了~~~')
})
//pipe()可以将可读流中的数据直接输出到可写流
rs.pipe(ws)
```