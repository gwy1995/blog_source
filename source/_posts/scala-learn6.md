---
title: scala学习笔记6
tags: scala
date: 2018-09-19 16:45:05
---


## 类

### 定义私有构造体

```scala
class Person private(name:String)

//error
val p = new Person("Mercedes")
```

### 构造体参数的默认值
```scala
class Socket(val timeout:Int = 10000)

val s = new Socket
s.timeout

val s = new Socket(5000)
s.timeout
```

### 覆写默认的getter和setter方法
```scala
class Person(private var _name: String){
	def name=_name
	def name_=(aName: String){_name=aName}
}
val p=new Person("Jonathan")
p.name="Jony"
println(p.name)
```
<!-- more -->
### 防止生成getter和setter方法
```scala
class Stock{
	// getter 和 setter方法自动生成
	var delayedPrice: Double= _

	// 使字段对其他类不可见
	private var currentPrice: Double= _
}

class Stock{
	//私有变量可以被任何Stock实例访问
	private var price:Double = _
	//若改为private[this]则其他实例不可见，只有当前实例可见
	// private[this] var price:Double= _
	def setPrice(p:Double){price=p}
	def isHigher(that:Stock):Boolean=this.price>that.price
}

val s1= new Stock
s1.setPrice(20)

val s2=new Stock
s2.setPrice(100)

println(s2.isHigher(s1))
```

### 使用代码块或函数定义字段
```scala
class Foo {
	// set 'text' equal to the result of the block of code
	val text = {
		var lines = "" 
		try {
			lines = io.Source.fromFile("/etc/passwd").getLines.mkString
		} 
		catch {
			case e: Exception => lines = "Error happened" 
		}
		lines
	}
	println(text)
}


class Foo {
	import scala.xml.XML
	// assign the xml field to the result of the load method
	val xml = XML.load("http://example.com/foo.xml")
}
```

### 设置未初始化的var变量类型
对于一些确定的类型，如字符或数值型，我们使用默认初始值，而一般的变量，我们使用Option.
```scala
case class Address(city: String, state: String, zip: String)
case class Person(var username: String, var password: String) {
	var age = 0
	var firstName = ""
	var lastName = ""
	var address = None: Option[Address]
}
val p = Person("alvinalexander", "secret")
p.address = Some(Address("Talkeetna", "AK", "99676"))
p.address.foreach { a => 
	println(a.city)
	println(a.state)
	println(a.zip)
}
//使用foreach是为了防止address为None时报错，若为None则foreach代码块将跳过，若为Some[Address]，则foreach循环将运行。
```

### 拓展类时的构造体变量
```scala
case class Address (city: String, state: String)
class Person (var name: String, var address: Address) {
	override def toString = if (address == null) name else s"$name @ $address"
}

class Employee (name: String, address: Address, var age: Int) extends Person (name, address) {
      // rest of the class
}
val teresa = new Employee("Teresa", Address("Louisville", "KY"), 25)
```
仅对于拓展类中新的变量进行val或var修饰。

### 调用超类构造体
```scala
// 主构造器
class Animal (var name: String, var age: Int) {
	//辅助构造器
	def this (name: String) {
		this(name, 0)
	}
	override def toString = s"$name is $age years old"
}

//调用一个变量的构造器
class Dog (name: String) extends Animal (name) {
	println("Dog constructor called")
}

//调用两个变量的构造器
class Dog (name: String) extends Animal (name, 0) {
	println("Dog constructor called")
}
```
子类的主构造器可以调用父类的主构造器或辅助构造器，但子类的辅助构造器无法调用到父类的辅助构造器。