[TOC]

# :dog:基础知识深入
## 一、数据类型
### 1、基本（值）类型
`
String:任意字符串
Number：任意数字
Boolean：true/false
undefined：undefined
null：null
`

### 2、对象（引用）类型
```
Object:任意对象
Function：一种特别的对象（存在代码数据，可以执行）
Array：一种特别的对象（数值下标，内部数据是有序的）
```
### 3、判断方法
```
typeof：不能判断null与object，array与object
instanceof:一般用来判断对象的具体类型
===
```
### 4、相关问题
#### 1. undefined与null的区别？
```
undefined代表定义了未赋值
null定义并赋值，只是值为空
```
#### 2.什么时候需要赋值为null？
```
初始赋值，表明将要赋值为对象
结束前，让对象成为垃圾对象(让垃圾回收器回收)
```
#### 3.严格区别变量类型和数据类型
变量类型（变量在内存栈中值的类型）：基本类型（数据本身）、引用类型（地址值）
数据类型（数据在堆或栈中的类型）：基本类型、对象类型
## 二、数据、变量、内存
### 1、定义
```
数据：存储在内存中代表特定信息的‘东东’，本质是‘0101010’二进制码
内存：内存条用来保存数据的空间（介质）
变量：可以变的量，由变量名和变量值组成（本质是一个键值对）
```
### 2、三者关系
```
内存是用来存储数据的临时空间，变量是内存的标识与数据的表示和使用
```
### 3、相关问题
> 在js调用函数时传递变量参数时，是值传递还是引用传递？
```
值传递(基本值|地址值)
```
> js引擎如何管理内存？
```
1.内存生命周期
分配小内存空间，得到它的使用权
存储数据，可以反复进行操作
释放小内存空间

2.释放内存
局部变量：函数执行完自动释放
对象：先成为垃圾对象，之后由垃圾回收器回收
```
## 三、对象
### 1、什么是对象？
多个数据的封装体，用来保存多个数据的容器，一个对象代表现实中的一个事物
### 2、为什么要用对象
统一管理多个数据
### 3、对象的组成？
属性：属性名（字符串）和属性值（任意类型）
方法：一种特殊的属性（属性值是function类型）
### 4、如何访问对象内部数据？
对象名.属性名：编码简单，有时不能用
1.属性名里含有特殊字符或空格时，不能用
2.属性名不确定，保存在一个变量里面时，不能用
对象名['属性名']：编码复杂，通用
## 四、函数（最复杂）
### 1、什么时函数？
实现特定功能的n条语句的集合，可以执行
### 2、为什么用函数？
提高代码复用，便于阅读
### 3、如何定义函数？
声明式
表达式

### 4、如何执行函数？
直接调用
对象调用
new调用
call | apply调用

### 5、回调函数
#### 常见回调函数
1. dom事件回调函数
2. 定时器回调函数
3. ajax请求回调函数
4. 生命周期回调函数
### 6、IIFE（匿名函数自调用） 
>Immediately-Invoked Function Expression  立即执行函数表达式
```javascript
;(function(){
    var a = 1
    function test(){ 
        console.log(++a)
    }
    window.$ = function(){//向外暴露一个全局函数$,该函数返回一个对象
        return{
            test:test
        }
    }
})()
//访问a的值
$().test()
```
# :cat:函数高级
## 一、原型与原型链
### 1、函数的prototype属性
> 每个函数都有一个prototype属性，它默认指向一个Object空对象（即：原型对象）
```
fun.prototype = {}
```
>原型对象中都有一个属性constructor，它指向函数对象
```
Date.prototype.constructor === Date
```
>给原型对象添加属性（一般都是方法）：函数的所有实例对象自动拥有原型对象中的属性|方法
```
fun.prototype.fun1 = function(){}
```
### 2、显式原型和隐式原型

1. 每一个函数对象都有一个prototype，即显式原型
2. 每一个实例对象都有一个proto，即隐式原型
3. 对象的隐式原型的值为其构造函数的显式原型的值
4. 所有函数对象的隐式原型proto都一样，指向Function.prototype 
5. ES6之前，程序员可以直接操作显式原型，但不能操作隐式原型
### 3、原型链（隐式原型链）
`访问一个对象属性时，现在自身属性中查找，找到返回。
如果没有找到，在沿着proto这条链向上查找，找到返回。
如果最终没有找到，返回undefined。
`

### 4、原型链——属性问题
1. 读取对象属性时：会自动到原型链中查找
2. 设置对象属性值时：不会查找原型链，如果当前没有此属性，直接添加并设置
3. 方法一般定义在原型中，属性一般通过构造函数定义在对象本身上
### 5、探索instanceof
```
1.instanceof是如何判断的？
A instanceof B
如果B函数的prototype在A的隐式原型链上，返回true，否则返回false 
2.Function是通过new自己产生的实例
```
### 6、面试题
> 试题1
```javascript
function A (){}
A.prototype.n = 1
var b = new A()
A.prototype = {n:2,m:3}
var c = new A()
console.log(b.n, b.m, c.n, c.m) //1,undefined,2,3
```
>试题2
```javascript
function F (){}
Object.prototype.a = funtion(){
    console.log('a方法')
}
Function.prototype.b = function(){
    console.log('b方法')
}
var f = new F()
f.a()// a方法
f.b()// ERR：b not a function
F.a()// a方法
F.b()// b方法
```
## 二、执行上下文与执行上下文栈
### 1、变量声明提升
* 通过var定义的变量，在定义语句之前就可以访问到
* 值：undefined 
```javascript
var a = 3
function fn(){
	console.log(a)// undefined
	var a = 4 //函数内部的a会被提升到函数开头，所以全局变量a不会在函数内部生效
}
fn()
```
### 2、函数声明提升
* 通过function声明的函数，在之前就可以直接调用
* 值：函数定义（对象）
```javascript
fn2()//fn2函数
function fn2(){
	console.log('fn2函数')
}
```
### 3、变量提升和函数提升是如何产生的？
> js引擎在执行代码之前会对全局上下文和函数上下文进行预处理，照成变量提升的现象
```javascript
fn3()// 不能调用，变量提升
var fn3 = function(){
	console.log('fn3函数')
}
```
### 4、代码分类
* 全局代码
* （函数）局部代码
### 5、全局执行上下文
* 在执行全局代码前将window确定为全局执行上下文
* 对全局数据进行**预处理**
	* var定义的全局变量==>undefined,添加为window属性
	* function声明的全局函数==>赋值(fun)，添加为window方法
	* this ==>赋值（window）
* 开始执行全局代码
### 6、函数执行上下文
* 在调用函数时，准备执行函数体***之前***，创建对应的函数执行上下文对象(虚拟的,存在于栈中)
* 对局部数据进行预处理
	* 形参==>赋值(实参),添加为执行上下文属性
	* arguments:实参列表(伪数组),添加为执行上下文属性
	* var定义的全局变量==>undefined,添加为执行上下文属性
	* function声明函数==>赋值(fun),添加为执行上下文方法
	* this==>赋值(调用函数对象)
* 开始执行函数整体代码
### 7、执行上下文栈
1. 在全局代码执行之前,JS引擎就会创建一个***栈***来保存管理所有的执行上下文对象
2. 在全局执行上下文(window)确定后,将其压入栈中
3. 在函数执行上下文创建后,将其压入栈中
4. 在当前函数执行完后,将其移除栈
5. 当所有函数代码执行完后,栈中只有window
> 题目1
```javascript
console.log('window-i'+i)
var i = 1
foo(1)
function foo(i){
	if(i== 4) return
	console.log('foo-begin'+i)
	foo(i+1)
	console.log('foo-end'+i)
}
console.log('window-end'+i)
```
>题目2
```javascript
//先执行变量声明提升,在执行函数声明提升
function a(){}
var a
console.log(typeof a)//function
```
>题目3
```javascript
//在 if/else语句中的var变量也会执行变量声明提升
if(!(b in window)){
	var b = 1
}
console.log(b)// undefined
```
>题目4
```javascript
//先执行变量声明提升,在执行函数声明提升,提升之后再执行变量赋值操作和函数执行操作,所以在赋值之后c已经不再是一个函数了,不能执行
var c = 1
function c (c){
	console.log(c)
	var c = 3
}
c(2)// TypeError:c is not a function
```
## 三、作用域与作用域链
### 1 .作用域
> 理解
* 就是一块"地盘",一个代码片段所在的区域
* 它是**静态的**(相当于上下文对象),在编写代码时就确定了
> 分类
* 全局作用域
* 函数作用域
* 块作用域
> **作用**
* **隔离变量**,不同的作用域下同名变量不会起冲突
### 2. 作用域与上下文
> 区别
* 全局执行上下文环境是在全局作用域确定之后,js代码马上执行之前创建生成的
* 函数作用域在函数定义时就已经确定了,而不是函数执行时
* 函数执行上下文环境是在函数调用时,函数代码执行前创建生成的
* 作用域是**静态的**,只要函数定义好就一直存在,不会发生变化
* 执行上下文是**动态的**,调用函数时创建,函数执行完自动释放
### 3.作用域链
> 嵌套作用域时,由内到外的作用域,形成作用域链,当查找变量时会沿着该链查找
### 4.题目
> 题目1
```javascript
var x = 10
function fun (){
	console.log(x)
}
function show(f){
	var x = 20
	f()
}
show(fun)// 10
```
> 题目2
```javascript
var fn = function(){
	console.log(fn)
}
fn()// 输出函数代码
var obj = {
	fn2:function(){
		console.log(fn2)
	}
}
obj.fn2()// ERR:fn2 is not defined
```
## 四、闭包
### 1. 如何产生闭包?
> 当一个嵌套的内部(子)函数引用嵌套外部(父)函数时,就产生了闭包.
### 2. 闭包到底是什么?
> 使用chrome调试查看
*  理解一: 闭包是嵌套在函数内部(大多数人)
*  理解二:包含被引用变量(函数)的对象(极少数人)
*  注意:闭包存在于嵌套的函数内部
### 3. 产生闭包的条件.
>函数嵌套
>内部函数引用外部函数的数据(变量/函数)
```javascript
function fn1 (){
	var a = 2
	function fn2 (){// 执行函数定义就会产生闭包,不用调用函数
		console.log(a)
	}
	fn2()
}
fn1()
```
### 4.常见的闭包
> 将函数作为另一个函数的返回值
```javascript
//共产生一个闭包,并赋值给变量f
function fn1(){
	var a = 2
	function fn2(){
		a++
		console.log(a)
	}
	return fn2
}
var f = fn1()
f()// 3
f()// 4
```
> 将函数作为实参传递给另一个函数调用
```javascript
function showDelay(msg,time){
	setTimeout(function(){
		alert(msg)
	},time)
}
showDelay('bibao',1000)
```
### 5.闭包的作用
1. 使用函数内部的变量在函数执行完后,仍然存活在内存中(延长了局部变量的生命周期)
2. 让函数外部可以操作到函数内部的数据(变量/ 函数)
> 问题
```
函数执行完后,函数内部声明的局部变量是否还存?
正常不存在,但可以通过闭包延长变量的生命周期
在函数外部能直接访问函数内部的局部变量吗?
不能,但可以通过闭包间接访问
```
### 6.闭包的生命周期
* 产生:在嵌套内部函数定义执行完成时就产生了(函数声明提升)
* 死亡:在嵌套的内部函数成为垃圾对象时死亡
### 7. 闭包的缺点及解决
>缺点
* 函数执行完后,函数内部的局部变量没有释放,占用内存事件边长
* 容易造成内存泄漏
>解决
* 能不用闭包就不用
* 及时释放
### 8. 内存溢出与内存泄露
> 内存溢出
* 一种程序运行出现的错误
* 当程序运行需要的内存超过剩余内存时,就会抛出内存溢出的错误/
> 内存泄漏
* 占用的内存没用及时的释放
* 内存泄漏积累的多了就容易造成内存溢出
* 常见的内存泄露
	* 意外的全局变量
	* 没有及时清理计数器或回调函数
	* 闭包
### 9.面试题
> 题目1
```javascript
var name = 'The window'
var object ={
	name:'My Object'
	getNameFun:function(){
		return function(){
			return this.name
		}
	}
}
alert(object.getNameFun())// 返回函数
alert(object.getNameFun()())// The window
```
>  题目2
```javascript
var name = 'The window'
var object ={
	name:'My Object'
	getNameFun:function(){
		var that = this
		return function(){
			return that.name
		}
	}
}
alert(object.getNameFun())// 返回函数
alert(object.getNameFun()())// My Object
```
>题目3
```javascript
function fun(n,o){
	console.log(o)
	return {
		fun:function(m){
			return fun(m,n)
		}
	}
}
//关注闭包的生存状态以及闭包里面保存的数据
//关键是有没有产生新的闭包，以及有没有使用新的闭包
//闭包 a,b,c
var a = fun(0)// undefined
a.fun(1)//0
a.fun(2)//0
a.fun(3)//0
var b = fun(0).fun(1).fun(2).fun(3)//undefined 0 1 2
var c = fun(0).fun(1)//undefined 0
c.fun(2)//1
c.fun(3)//1
```
# :frog:面向对象高级
## 一、对象创建模式
### 1、Object构造函数模式
* 方式：先创建空Object对象，再添加属相/方法
* 适用场景：起始不确定对象内部数据
* 问题：语句太多
```javascript
var p =  new Object()
p.name = 'Tom'
p.age = 20
p.setName = function(name){
	this.name = name
}
```
### 2、对象字面量
* 方式：使用{}创建对象，同时指定属相/方法
* 适用场景：起始对象内部数据确定
* 问题：如果创建多个对象，有重复代码
```javascript
var p =  {
	p.name = 'Tom'
	p.age = 20
	p.setName = function(name){
		this.name = name
	}
}
```
### 3、工厂模式
* 方式：通过工厂函数创建对象并返回
* 适用场景：需要创建多个对象
* 问题：对象没有一个具体的类型
```javascript
function createPerson( name,age){
	var p =  {
	p.name = 'Tom'
	p.age = 20
	p.setName = function(name){
		this.name = name
	}
	return p
}
var p1 = createPerson('Tom',20)
```
### 4、自定义构造函数模式
* 方式：自定义构造函数，通过new创建对象
* 适用场景：需要创建多个确定类型的对象
* 问题：每个对象都有相同的数据（主要指方法），浪费内存
```javascript
// 定义构造函数
function Person( name,age){
	this.name = name
	this.age =age
	this.setName = function(name){
		this.name =name
	}
}
var p1 = new Person('Tom',20)
var p2 = new Person('Jack',30)
//p1,p2都具有自己的setName方法，浪费内存
```
### 5、构造函数+原型组合模式(实际开发常用)
* 方式：自定义构造函数，属性在函数中初初始化，方法添加到原型上
* 适用场景：需要创建多个确定类型的对象
```javascript
// 定义构造函数
function Person( name,age){
	this.name = name
	this.age =age
}
Person.prototype.setName = function(name){
	this.name = name
}
var p1 = new Person('Tom',20)
var p2 = new Person('Jack',30)
```
## 二、继承模式
### 1、原型链继承
> 方式
1. 定义父类型构造函数
2. 给父类型原型添加方法
3. 定义子类型的构造函数
4. 创建父类型对象赋值给子类型的原型
5. 将子类型的构造函数属性设置为子类型
6. 给子类型原型添加方法
7. 创建子类型对象，可以调用父类型方法
> 关键：**子类型的原型为父类型的一个实例**
```javascript
// 父类型
function Supper(){
	this.supProp = 'Supper property'
}
Supper.prototype.showSupperProp = function(){
	console.log(this.supProp)
}
// 子类型
function Sub(){
	 this.subProp = 'Sub property'
}
//关键：创建父类型对象赋值给子类型的原型
Sub.prototype = new Supper()
//将子类型的构造函数属性设置为子类型
Sub.prototype.constructor = Sub
Sub.protopety.showSubProp = function(){
	console.log(this.subProp)
}
var sub = new Sub()
sub.showSubProp()
sub.showSupperProp()
```
### 2、借用构造函数继承(假的)
> 方式
1. 定义父类型构造函数
2. 定义子类型构造函数
3. 在子类型构造函数中调用父类的构造函数
> 关键：**在子类型构造函数中调用父类的构造函数**
```javascript
function Person(mame,age){
	this.name = name
	this.age = age
}
function Student(name,age,price){
	Person.call(this,name,age)
	this.price = price
}
```
### 3、组合继承
* 利用原型链实现对父类型任意方法的继承
* 利用call()借用父类型的构造函数初始化相同属性
```javascript
// 父类型
function Person(name,age){
	this.name = name
	this.age = age
}
Person.prototype.setName = function(name){
	this.name = name
}
// 子类型
function Student(name,age,price){
	Person.call(this,name,age)
	this.price = price
}
//关键：创建父类型对象赋值给子类型的原型
Student.prototype = new Person()
//将子类型的构造函数属性设置为子类型
Student.prototype.constructor = Student
Student.protopety.setPrice = function(price){
	this.price = price
}
var student = new Student('Tom',20,1000)
student.setName('Jack')
student.setPrice(2000)
```
# :bear:线程机制与事件机制
## 一、进程与线程
### 1、进程
> 程序的一次执行，它占有一篇独立的内存空间
> 可以通过windows任务管理器查看进程
### 2、线程
> 线程是进程内一个独立执行单元
> 是程序执行的一个完整流程
> 是CPU的最小调度单元
### JS线程
> js是单线程运行的
> 但是使用H5中的Web Workers可以多线程运行
### 浏览器
> 浏览器是多线程
> 浏览器有多进程的也有单进程的
## 二、浏览器内核 
> 支持浏览器运行的最核心程序
### 1.不同浏览器的内核可能不一样
* Chrome,Safari:webkit
* firefox:Gecko
* IE:Trident
* 360,搜狗等国内浏览器:Trident+webkit
### 2. 内核有很多模块组成
* 主线程
	* js引擎模块:负责js程序的编译与运行
	* html,css文档解析模块:负责页面文本的解析
	* DOM/CSS模块:负责dom/css在内存中的相关处理
	* 布局和渲染模块:负责页面布局和效果的绘制
* 分线程
	* 定时器模块:负责定时器管理
	* DOM事件响应模块:负责事件管理
	* 网络请求模块:负责ajax请求
## 三、定时器引发的思考
### 1.定时器真的是定时执行吗?
* 定时器并不能保证正真的定时执行
* 一般会延长一点(可以接受),也可能延迟很长时间(不能接受)
```javascript
var start = Date.now()
setTimeout(function(){
	console.log('定时器执行了,间隔'+(Date.now()-start))
},200)
```
### 2. 定时器回调函数在分线程执行吗?
* 在主线程上执行的,js是单线程
### 3.定时器是如何实现的?
* 事件循环模型
## 四、事件轮询（驱动）模型
### 1. 代码分类
* 初始化代码（同步代码）：包含了dom事件绑定、设置定时器、发送ajax请求
* 回调执行代码（异步代码）：处理回调逻辑
### 2.js引擎执行代码流程
* js引擎再主线程中执行，且单线程执行
* 先执行初始化代码，初始化代码执行完后才能执行回调代码
### 3.模型的两个分类
* 事件管理（dom/定时器/ajax）模块
* 回调队列
### 4.模型的运转流程
* js引擎先（主线程）执行初始化代码，将事件回调函数交给对应的事件管理模块
* 当事件发生时，事件管理模块（分线程）将回调函数及其数据添加到回调**队列**中
* 只用当初始化代码执行完，（可能要一定的时间），才会循环遍历回调队列中的回调函数，放入执行栈中执行（主线程）
### 5.事件驱动模型
> 整个事件执行过程（流程）就是事件驱动，所以该模型也成为事件驱动模型

## 五、H5 Web Workers（多线程）
### 1.H5规范提供了js分线程的实现，取名为：Web Workers
* 我们可以将一些计算量比较大的代码交给Web Worker运行而不懂结用户界面
* 但是分线程完全受主线程控制，而且**不能操作dom**，所以并没有改变js是单线程的本质
### 2. 相关API
* Worker：构造函数，加载分线程执行的js文件
* Worker.prototype.onmessage:用于接收另一个线程的回调函数
* Worker.prototype.postMessage:向另一个线程发送消息
### 3. 不足
* worker内的代码不能操作dom
* 不能跨域加载js
* 不是每个浏览器都支持这个新特性
### 4. 使用
* 创建在分线程执行的js文件
* 在主线程中的js中发送消息并设置回调
```javascript
var input = document.getElementById('ipt')
document.getElementById('btn').onclick = function(){
    var number = input.value
    //创建一个Worker对象，传一个js文件路径
    var worker = new Worker('worker.js')
    //主线程向分线程传递数据
    worker.postMessage(number)
    //绑定接收消息的监听处理函数（回调代码），类似于事件处理函数
    worker.onmessage = function(event){
        console.log('主线程接收分线程返回的数据')
        alert(event.data)
    }
}
```
```javascript
//worker.js
function fibonacci(n){
     return n<=2 ? 1 : fibonacci(n-1)+fibonacci(n-2)
 }
var onmessage = function(event){
	console.log('分线程接收到主线程的数据：'+event.data)
	var result = fibonacci(event.data)
	//分线程向主线程返回数据
	postMessage(result)
}
```

