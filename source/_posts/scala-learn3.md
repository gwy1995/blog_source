---
title: scala学习笔记3
tags: scala
date: 2018-08-16 16:26:23
---


## 控制结构

### 循环

for循环
```scala
val a=Array("apple","banana","orange")
for (e <- a) println(e)
for (e <- a){
	val s=e.toUpperCase
	println(s)
}

// 使用for循环返回值
val newArray=for (e <- a) yield e.toUpperCase
val newArray=for (e<-a) yield{
	val s=e.toUpperCase
	s
}

// for循环计数器
for (i <- 0 until a.length){
	println(s"$i is ${a(i)}")
}
for ((e,count)<- a.zipWithIndex){
	println(s"$count is $e")
}

```
<!-- more -->
```scala
// 生成器中加入if语句
1 to 3
for (i<-1 to 3) println(i)
for (i<-1 to 10 if i!=4) println(i)

// Map对象的遍历
val names=Map("fname"->"Robert","lname"->"Goren")
for ((k,v)<- names) println(s"key:$k,value:$v")

// for each语句
a.foreach(println)
a.foreach(e => println(e.toUpperCase))
a.foreach{
	e=>
	val s=e.toUpperCase
	println(s)
}

// for循环使用多重计数器
for (i<- 1 to 3;j<-1 to 2)println(s"i=$i,j=$j")

// 好看一点的形式
for {
	i <- 1 to 3
	j <- 1 to 5
	k <- 2 to 4
} println(s"i=$i,j=$j,k=$k")

// for循环中加入if语句
for {
	i <- 1 to 10
	if i%2==0
	if i>3
	if i<8
} println(i)

// 列表推导
val names = Array("chris", "ed", "maurice")
val capNames = for (e <- names) yield e.capitalize
val upNames = names.map(_.toUpperCase)
```

