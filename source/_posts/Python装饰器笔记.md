---
title: Python装饰器笔记
intro: 装饰器，Python的语法糖
tags: Python
comments: true
permalink: 1516704438
date: 2018-01-23 18:47:18
---
装饰器，Python的语法糖
<!-- more -->
------
Python装饰器的作用是使函数包装与方法包装（一个函数，接受函数并返回其增强函数）变得更容易阅读和理解。 最初的使用场景是在方法定义的开头能够将其定义为类方法或静态方法。如果不用装饰器语法的话，定义可能会非常稀疏，并且不断重复：
```python3
 class WithoutDecorators:
    def some_static_method():
      print("this is a static method")
    some_static_method = staticmethod(some_static_method)
    def some_static_method(cls):
      print("this is a class method")
    some_class_method = classmethod(some_class_method)
```

如果用装饰器语法重写的话，代码会更加简短，也更容易理解:
```python3
class WithoutDecorators:
    @staticmethod
    def some_static_method():
      print("this is a static method")

    @classmethod
    def some_class_method(cls):
      print("this is a class method")
```
-------------
####   1.一般语法和可能实现

装饰器通常是一个命名对象（不允许使用lambda表达式），在被（装饰函数）调用时接受单一参数，并返回另一个可调用对象，任何可调用对象（任何实现了__call__方法的对象都是可以调用的）都可以用作装饰器，它们返回的是实现了自己的__call__方法的更复杂的类的实例。

######    (1)作为一个函数
通用函数如下:
```python3
def mydecorator(function):
  def wrapped(*args,**kwargs):
    #在调用原始函数之前做点什么
    result=function(*args,**kwargs)
    #在函数调用之后，做点什么
    return result
  return wrapped
```

######    (2)作为一个类
通用模式如下：
```python3
class DecoratorAsClass:
  def __init__(self,function):
    self.function=function

  def __cal__(self,*args,**kwargs):
    #在调用原始函数之前做点什么
    result=function(*args,**kwargs)
    #在函数调用之后，做点什么
    return result
```

######    (3)参数化装饰器
参数化的装饰器通常需要用到第二层包装，下面举一个简单的装饰器实例，给定重复次数，每次数调用时都会重复执行一个装饰函数：
```python3
def repeat(number=3):
  '''多次重复执行装饰函数，返回最后一次原始函数调用的值作为结果
  :param number:重复次数，默认值是3
  '''
  def actual_decorator(function):
    def wrapped(*args,**kwargs):
      result = None
      for _ in range(number):
        result = function(*args,**kwargs)
      return result
    return wrapped
  return actual_decorator
```
如：
```python3
@repeat(2)
def foo():
  print("foo")
```
当调用foo方法时，输出两个"foo"。注意，即使参数化装饰器的参数有默认值，名字后面也必须加括号。

######   (4)保存内省的装饰器
使用装饰器的常见错误是在使用装饰器时不保存函数元数据（主要是文档字符串和原始函数名）。装饰器组合创建了一个新函数，并返回一个新对象，但却完全没有考虑原始函数的标识。这会使得调试这样装饰过的函数更加困难，也会破坏可能用到的大多数自动生成文档的工具，因为无法访问原始的文档字符串和函数签名。解决这个问题的方法，可以使用functools模块内置的wraps()装饰器。
```python3
from functools import wraps

def preserving_decorator(function):
  @wraps(function)
  def wrapped(*args,**kwargs):
    """包装函数内部文档"""
    return function(*args,**kwargs)
  return wrapped

@preserving_decorator
def function_with_important_docstring():
  """这是我们想要保存的重要文档字符串"""
```
