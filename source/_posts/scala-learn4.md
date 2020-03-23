---
title: scala学习笔记4
tags: scala
date: 2018-08-24 13:31:45
---


## 控制结构

### if类三元表达式
```scala
val absValue=if (a<0) -a else a
```
<!-- more -->
### match表达式
```scala
val i=10
i match {
	case 1 => println("January")
	case 2 => println("February")
	case 3 => println("March")
	case 4 => println("April")
	case 5 => println("May")
	case 6 => println("June")
	case 7 => println("July")
	case 8 => println("August")
	case 9 => println("September")
	case 10 => println("October")
	case 11 => println("November")
	case 12 => println("December")
	case whoa => println("Unexpected case: " + whoa.toString)
}

// 将match返回值赋值
val month = i match {
	case 1 => "January"
	case 2 => "February"
	case 3 => "March"
	case 4 => "April"
	case 5 => "May"
	case 6 => "June"
	case 7 => "July"
	case 8 => "August"
	case 9 => "September"
	case 10 => "October"
	case 11 => "November"
	case 12 => "December"
	case _ => "Invalid month" // the default, catch-all
}

// default 
// 若无需赋值，则使用_
case _ => println("Got a default match")
// 需要赋值，则输入一个变量
case default => println(default)

// 可以匹配变量类型
val x:Any="a"
x match { 
	case s: String => s + " is a String"
	case i: Int => "Int"
	case f: Float => "Float"
	case l: List[_] => "List"
	case _ => "Unknown"
}

// 匹配多个值
val i = 5 
i match {
	case 1 | 3 | 5 | 7 | 9 => println("odd")
	case 2 | 4 | 6 | 8 | 10 => println("even") 
}

// 模式匹配
def echoWhatYouGaveMe(x: Any): String = x match {
	// 常数模式
	case 0 => "zero"
	case true => "true"
	case "hello"=> "you said 'hello'"
	case Nil => "an empty list"

	// 序列模式
	case List(0,_,_) => "a three-element list with 0 as the first element"
	case List(1,_*)=> "a list beginning with 1,having any number of elements"
	case Vector(1,_*)=> "a vector starting with 1,having any number of elements"

	// 元组
	case (a,b)=> s"got $a and $b"
	case (a,b,c)=> s"got $a,$b,and $c"

	// 构造体模式
	case Person(first,"Alexander") =>s"found an Alexander,first name=$first"
	case Dog("Suka")=> "found a dog named Suka"

	// 类型模式
	case s: String =>s"you gave me this string: $s"
	case i: Int =>s"thanks for the int: $i"
	case f: Float=>s"thanks for the float: $f"
	case a: Array[Int]=>s"an array of int:${a.mkString(",")}"
	case as:Array[String]=>s"an array of strings:${as.mkString(",")}"
	case d:Dog=>s"dog:${d.name}"
	case list: List[_]=>s"thanks for the list:$list"
	case m:Map[_,_]=>m.toString

	// 默认通配符模式
	case _=> "Unknown"
}
```
