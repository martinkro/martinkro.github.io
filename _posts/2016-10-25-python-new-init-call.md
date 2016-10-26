---
layout: post
title: "Python 特殊函数 __init__,__new__,__call__"
date: 2016-10-25
---

```python
# 1.py
class Test(object):

    def __new__(cls, *args, **kwargs):
        print("new")
        return super(Test,cls).__new__(cls,*args,**kwargs)

    def __init__(self, *args, **kwargs):
        print("init")
        super(Test,self).__init__(*args,**kwargs)

    def __call__(self, *args, **kwargs):
        print("call")

obj=Test()
obj()
```

```powershell
> python 1.py
new
init
call
```

注：

- __new__在创建对象时调用，返回当前对象的一个实例。第一个参数是cls即class本身。
- __init__在创建完对象后调用，可以进行对象实例的初始化操作，无返回值。第一个参数是self，表示实例对象。
- __call__如果类实现了这个方法，则实例是可调用的，相当于重载了括号运算符。

## __init__函数

当一个类实例被创建时， __init__() 方法会自动执行，在类实例创建完毕后执行，类似构建函数。__init__() 可以被当成构建函数，不过不象其它语言中的构建函数，它并不创建实例--它仅仅是你的对象创建后执行的第一个方法。它的目的是执行一些该对象的必要的初始化工作。通过创建自己的 __init__() 方法，你可以覆盖默认的 __init__()方法（默认的方法什么也不做），从而能够修饰刚刚创建的对象__init__()需要一个默认的参数self，相当于this。

## __call__函数

Python中有一个有趣的语法，只要定义类型的时候，实现__call__函数，这个类型就成为可调用的。　 

换句话说，我们可以把这个类的对象当作函数来使用，相当于重载了括号运算符。为了弄明白python中__setattr__, __getattr__, __delattr__, __call__的作用，重写dict，扩展其功能。

```python
class storage(dict):  
#通过使用__setattr__, __getattr__, __delattr__  
#可以重写dict，使之通过“.”调用  
    def __setattr__(self, key, value):  
        self[key] = value  
    def __getattr__ (self, key):  
        try:  
            return self[key]  
        except KeyError, k:  
            return None  
    def __delattr__ (self, key):  
        try:  
            del self[key]  
        except KeyError, k:  
            return None  
  
# __call__方法用于实例自身的调用  
#达到()调用的效果  
    def __call__ (self, key):  
        try:  
            return self[key]  
        except KeyError, k:  
            return None  
  
s = storage()  
s.name = "hello"#这是__setattr__起的作用  
print s("name")#这是__call__起的作用  
print s["name"]#dict默认行为  
print s.name#这是__getattr__起的作用  
del s.name#这是__delattr__起的作用  
print s("name")  
print s["name"]  
print s.name  
```
