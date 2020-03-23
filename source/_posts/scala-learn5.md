---
title: scala学习笔记5
tags: scala
date: 2018-09-03 17:18:17
---

## 类

### 创建一个基本的构造体
```scala
class Person(var firstName:String,var lastName:String){
	println("the constructor begins")

	private val HOME=System.getProperty("user.home")
	var age=0

	override def toString =s"$firstName $lastName is $age years old"
	def printHome{
		println(s"HOME=$HOME")
	}
	def printFullName{
		println(this)
	}

	printHome
	printFullName
	println("still in the constructor")
}
```
<!-- more -->
### 控制结构体的可见字段
```scala
// 对于用var声明的字段，scala会为这个字段生成getter和setter方法
// 对于val声明的字段，scala会生成getter方法
// 如果字段没有使用val和var声明，scala会十分保守，getter和setter方法都不生成。
// 此外，可以使用private关键词来修改var和val的字段，它可以防止生成getter和setter字段。

// var字段
class Person(var name: String)
val p = new Person("Alvin Alexander")
// getter
p.name
// setter
p.name="Fred Flintstone"
p.name

// val字段
class Person(val name: String)
val p = new Person("Alvin Alexander")
// getter
p.name
// setter error
p.name = "Fred Flintstone"

// 不用val和var的字段
class Person(name: String)
val p = new Person("Alvin Alexander")
// getter error
p.name

// 给val和var字段增加private
class Person(private var name: String) { def getName {println(name)} }
val p = new Person("Alvin Alexander")
// getter
p.name //error
p.getName
```

#### 构造体字段设置的效果

| 字段能见度						 |  可以获取  |  可以修改  |
| ----     						| --- | ---   |
| var      						| yes| yes|
| val                			| yes| no|
| 默认没有var和val修饰            | no| no|
| 使用private关键词修饰var和val    | no| no|

### 定义辅助构造体

```scala
//主构造体
class Pizza (var crustSize: Int,var crustType: String){
	// 一个参数的辅助构造体
	def this(crustSize: Int){
		this(crustSize,Pizza.DEFAULT_CRUST_TYPE)
	}
	def this(crustType: String){
		this(Pizza.DEFAULT_CRUST_SIZE,crustType)
	}

	// 无参数的辅助构造体
	def this(){
		this(Pizza.DEFAULT_CRUST_SIZE,Pizza.DEFAULT_CRUST_TYPE)
	}

	override def toString = s"A $crustSize inch pizza with a $crustType crust"
}
// 伴生对象
object Pizza{
	val DEFAULT_CRUST_SIZE=12
	val DEFAULT_CRUST_TYPE="THIN"
}
val p1 = new Pizza(Pizza.DEFAULT_CRUST_SIZE, Pizza.DEFAULT_CRUST_TYPE) 
val p2 = new Pizza(Pizza.DEFAULT_CRUST_SIZE)
val p3 = new Pizza(Pizza.DEFAULT_CRUST_TYPE)
val p4 = new Pizza
```
