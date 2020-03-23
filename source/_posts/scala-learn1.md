---
title: scala学习笔记1
tags: scala
date: 2018-08-10 09:07:43
---


## String

```scala
// 字符串创建
val s = "Hello, world"

// 字符串长度
s.length

// 字符串可以使用foreach
s.foreach(println)

// 字符串拼接
val s1="hello"+"world"

// 全大写
s1.toUpperCase

```
<!-- more -->
```scala
// 多行字符串 
val s2="""
hello,
world
"""

// 多行字符串排列不整齐问题
val s3="""
hello,
|world
""".stripMargin

// 多行合并成一行
val s4="""
hello,
world
""".replaceAll("\n","")

// 分割字符串
val s5="hello , world"
s5.split(",")

//分割且去除多余空格
s5.split(",").map(_.trim)

//split支持正则
s5.split("l+")

//字符串中变量替换
val name="Fred"
val age=33
val weight=200.00
s"$name is $age years old, and weighs $weight pounds"

//也可以在字符串中使用表达式或者对象的属性
s"Age next year:${age+1}"
s"You are 33 years old:${age == 33}"
case class Person(name:String,age:Int) {}
val tom=Person("Tom",20)
s"${tom.name} is ${tom.age} years old"

// 构造正则表达式
val numPattern = "[0-9]+".r
// 或者
import scala.util.matching.Regex
val numPattern = new Regex("[0-9]+")

// 正则匹配
val address = "123 Main Street Suite 101"
val matches = numPattern.findAllIn(address)

// 不同的正则替换方式，使用字符串.replaceAll(正则表达式，xxx)
val address = "123 Main Street".replaceAll("[0-9]", "x")
// 或者使用正则表达式.replaceAllIn(字符串,xxx)
val regex = "[0-9]".r
val newAddress = regex.replaceAllIn("123 Main Street", "x")

// 多个正则串
val pattern = "([0-9]+) ([A-Za-z]+)".r
val pattern(count, fruit) = "100 Bananas"

// 字符串中取某个字符
"hello"(1)
// 截取子串
"hello".substring(0,2)

```
