---
title: scala学习笔记2
tags: scala
date: 2018-08-10 14:01:27
---


## Numbers

### scala数据类型的数值范围

| 数据类型 | 范围                      |
| ----   | ---                       |
| Char   | 16位无符号Unicode字符       |
| Byte   | 8位有符号数值               |
| Short  | 16位有符号数值              |
| Int    | 32位有符号数值              |
| Long   | 64位有符号数值              |
| Float  | 32位IEEE 754标准单精度浮点数 |
| Double | 64位IEEE 754标准单精度浮点数 |

<!-- more -->
```scala
// 获取类型的范围
Short.MaxValue
Float.MinValue

// 字符串转数值
"100".toInt
"100".toDouble
"100".toFloat
"100".toLong
"100".toShort
"100".toByte
//非数值型字符串转化会报错
"foo".toInt

// BigInt和BigDecimal可以直接在创建时转化
val b=BigInt("1")
val b=BigDecimal("3.14159")

// 进制转换
Integer.parseInt("B",16)
Integer.parseInt("100",2)

// 数值类型间相互转化
19.45.toInt
19.toFloat
19.toDouble
// isValid
val a=1000L
a.isValidByte
a.isValidShort

// +=-=*=/=
var a=1
a+=1
a-=1
a*=2
a/=2

// 浮点数的对比
val a=0.3
val b=0.1+0.2
a==b

// 随机数
val r = scala.util.Random
r.nextInt
// 100以内随机数
r.nextInt(100)
// 0.0-1.0之间的随机浮点数
r.nextFloat
// 设置随机数种子
val r = new scala.util.Random(100)
// 可以在创建Random对象后再设置随机数种子
r.setSeed(100)

// 创建Range,List,Array
val r= 1 to 10
val r= 1 to 10 by 2
// toArray toList
val x = 1 to 10 toArray
val x = 1 to 10 toList

// 数值格式化
val a=3.1415926
// 使用f字符串插值
f"$a%1.5f"
// 使用format方法
"%1.5f".format(a)
// 使用formatter增加逗号
val formatter = java.text.NumberFormat.getIntegerInstance
formatter.format(10000)
// 金额显示
val formatter = java.text.NumberFormat.getCurrencyInstance
formatter.format(123.456789)
```

