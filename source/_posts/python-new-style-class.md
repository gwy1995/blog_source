---
title: python中的new style class
tags: python
date: 2017-08-29 14:20:00
---

## python2中没有继承object的类是old-style-class
继承了object的类是new-style-class

对于一个 new-style 的类,它的实例的类型和类都是一致的

对于一个 old-style 的类，它的实例的类型是instance,类与类型不是一个含义

<!-- more -->
```python
class A(object):
    pass
class B:
    pass
```

A是new-style,B是old-style

[官方wiki](https://wiki.python.org/moin/NewClassVsClassicClass)

## 在python3中均为new style class
