### 一、Java内存被分为3类
#### 1. 栈空间
* 栈描述的是方法执行时的内存模型，每个方法被执行，都会创建一个栈帧
* 自动分配连续的空间，**速度快**
* **线程私有**，不能被线程之间共享
* 主要放置局部变量、操作数和方法出口

#### 2.堆空间	

* 堆是一个**不连续**的内存空间，分配灵活，**速度慢**
*  JVM**只有一个堆**，被所有线程共享
* 堆用于存储创建好的对象和数组(数组也是对象)

#### 3.方法区（静态区）

* **方法区实际也是堆**，只是用于存储类、常量相关的信息
* JVM只有一个方法区，被**所有线程**共享
* 用来存放程序中永远是不变或唯一的内容。(类信息[class]、静态变量、字符串常量等)

### 二、方法被调用时的内存分析

> 示例代码

```java
public class Student {
	String name;
	int age;
	public void study() {
		System.out.println("study");
	}
	public void changeName(String newName) {
		this.name = newName;
	}
}
public class TestStu {
	public static void main(String[] args) {
		Student stu = new Student();
		stu.name = "Tom";
		stu.age = 18;
		stu.study();
		stu.changeName("Jack");	
	}
}
```

> 过程分析

1. 编译后，JVM通过类加载器将TestStu.class加载到方法区（静态变量、静态块、静态方法、常量）
2. JVM调用main方法时，会***在栈空间开辟一块内存空间***（用来存放局部变量和被调用方法）
2. 执行到Student，JVM以同样的方式加载Student.class到方法区（静态变量、静态块、静态方法、常量）
2. 执行到stu时，stu在main方法内部，因而是局部变量，存放在栈空间中`stu入栈`
3. 执行到new Student()时，根据方法区的Student.class，创建实例对象,并`放入堆空间`
3. 最后通过`=`将堆空间的实例对象`地址`赋值给栈空间的stu变量
4. 执行stu.study()时，`study()入栈`。（方法都是以签名的形式存在的，只有在调用的时候分配内存空间）
5. 当study方法调用完成，`study()出栈`
6. 执行stu.changeName("Jack")时，`changeName()`入栈。（修改堆空间里面stu对象的name属性值）
7. 当chengeName方法调用完成，`changeName()出栈`
8. main方法执行结束，`stu出栈`。***释放在栈空间开辟的内存空间***

> 启示

* 同理，在执行study()和changeName()时也会开辟用栈空间来栈空间存放所需要的局部变量和函数调用。由此可以得出函数调用链的概念，具有层级的特点。由于函数调用也类似于栈的特点，也称为`函数调用栈`。
* 处于不同栈空间的局部变量可以同名，可以得出局部变量作用域的概念。即在当前栈空间找不到该变量时会到调用该函数的栈空间去找，依次类推，形成`局部变量作用域`。

