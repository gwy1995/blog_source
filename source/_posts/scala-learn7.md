---
title: scala学习笔记7
tags: scala
date: 2018-09-27 17:36:17
---


## 类

### 使用抽象类
`scala`语法中有`traits`(特征)，`trait`比`abstract class`(抽象类)复杂的多。

因此在scala中使用抽象类的场景不多，主要有两个场景:
- 需要创建一个有构造体变量的基础类
- 会被`java`代码调用
<!-- more -->
第一个场景是因为`traits`不允许构造体参数。
```scala
// 这将不能通过编译
trait Animal(name: String)

//这时需要使用抽象类
abstract class Animal(name: String)
```
第二个场景是因为`scala`带实现方法的`trait`不能被`Java`代码调用。如解决这个问题的方法在以后讨论。

需要知道的是：一个类只能被一个抽象类拓展。

### 在抽象类（或特征）中定义属性
```scala
abstract class Pet(name: String){
	val greeting: String
	var age: Int
	def sayHello { println(greeting) }
	override def toString = s"I say $greeting, and I'm $age"
}

class Dog (name:String) extends Pet(name){
	val greeting = "Woof"
	var age = 2
}

class Cat (name:String) extends Pet(name){
	val greeting = "Meow"
	var age = 5
}

val dog = new Dog("Fido")
val cat = new Cat("Morris")
dog.sayHello
cat.sayHello
println(dog)
println(cat)
```

### 使用Case Class生成样板代码

当使用`match`表达式，`actors`或其他场景时，我们需要是用`case class`来生成样板代码，使用`case class`能直接生成`setter`和`getter`方法，以及`apply`、`unapply`、`toString`、`equals`、`hashCode`等方法。

```scala
// name and relation are 'val' by default
case class Person(name: String, relation: String)
```
定义一个`case class`时，很多方法代码都会自动生成：
- `apply`方法，在生成类的实例时不需要使用`new`关键词
- 对于`val`变量自动生成取值方法，对于`var`变量自动生成取值和设置方法。
- 一个不错的`toString`方法
- `unapply`方法，可以使用`match`表达式
- `equals`和`hashCode`方法
- `copy`方法

```scala
case class Person(name: String, relation: String)

//不需要new关键词
val emily = Person("Emily", "niece")

//取值方法
emily.name

//对于var变量还会生成设置方法
case class Company (var name: String)
val c = Company("Mat-Su Valley Programming")
c.name
c.name = "Valley Programming"

//toString方法
emily

//unapply方法
emily match { case Person(n, r) => println(n, r) }

//equals和hashCode方法
val hannah = Person("Hannah", "niece")
emily == hannah

//copy方法
case class Employee(name: String, loc: String, role: String)
val fred = Employee("Fred", "Anchorage", "Salesman")
val joe = fred.copy(name="Joe", role="Mechanic")
```

### 定义一个`equals`方法
```scala
class Person (name: String, age: Int) {
	def canEqual(a: Any) = a.isInstanceOf[Person]
	override def equals(that: Any): Boolean = that match {
		case that: Person => that.canEqual(this) && this.hashCode == that.hashCode
		case _ => false
	}
	override def hashCode:Int = {
		val prime = 31
		var result = 1
		result = prime * result + age;
		result = prime * result + (if (name == null) 0 else name.hashCode)
		return result
	} 
}
```
上面是一种用`hashCode`方法定义`equals`方法。
有了`equals`方法，就可以用`==`比较实例
```scala
val nimoy = new Person("Leonard Nimoy", 82)
val nimoy2 = new Person("Leonard Nimoy", 82) 
val shatner = new Person("William Shatner", 82) 
val ed = new Person("Ed Chigliak", 20)
nimoy == nimoy
nimoy == nimoy2
nimoy2 == nimoy
nimoy != shatner
shatner != nimoy
nimoy != null
nimoy != ed
```

### 创建内部类
创建内部类可以其接触外部的API或者被其他代码引用。
```scala
class PandorasBox {
	case class Thing (name: String)
	var things = new collection.mutable.ArrayBuffer[Thing]() 
	things += Thing("Evil Thing #1")
	things += Thing("Evil Thing #2")
	def addThing(name: String) { things += new Thing(name) } 
}
val p = new PandorasBox
p.things.foreach(println)
p.addThing("Evil Thing #3")
p.addThing("Evil Thing #4")
```
在PandorasBox类外部是没有Thing这个类的，`val x=new Thing()`会报错。