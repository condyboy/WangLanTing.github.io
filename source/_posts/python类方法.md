---
title: python类方法
date: 2019-07-25 11:08:41
categories: python
tags:
---
# python方法
### 比较普通类方法、用@classmethod和@staticmethod修饰的类方法的区别

```
class A(object):
    def foo(self, x):
        print("executing foo(%s,%s)" % (self, x))
        print('self:', self)
    @classmethod
    def class_foo(cls, x):
        print("executing class_foo(%s,%s)" % (cls, x))
        print('cls:', cls)
    @staticmethod
    def static_foo(x):
        print("executing static_foo(%s)" % x)    
a = A()
```
1、普通类方法也称实例化方法（foo）只能够通过实例化对象进行调用，如`a.foo(1)`，它通过**self**来传递当前类对象的实例
2、用@classmethod修饰的类方法（class_foo相当于类中的静态方法）可以直接通过类名调用，它通过**cls**来传递当前类对象，也可以通过实例化对象调用，如`A.class_foo(1) `
`a.class_foo(1)`
3、用@staticmethod修饰的类方法和普通的类外方法相同，只是将函数嵌入到类中，只是没有传递类对象或类实例对象，通过类对象或者类实例调用，
如`A.class_foo(1) `
`a.class_foo(1)`

