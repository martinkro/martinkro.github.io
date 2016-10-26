---
layout: post
title: "Python 设计模式：单例模式"
date: 2016-10-24
---

{:toc}

> 单例模式，也叫单子模式，是一种常用的软件设计模式。在应用这个模式时， 单例对象的类必须保证只有一个实例存在。许多时候整个系统只需要拥有一个 全局对象，这样有利于我们协调系统整体的行为。 －－以上来自维基百科

从定义上来看，这会是一个很有用的避免冲突的设计模式，相当于把所有同样资源的调用 都交给了一个资源代理。那么 Python 中该如何实现这一模式呢？

单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。
单例模式的要点有三个；
- 一是某个类只能有一个实例；
- 二是它必须自行创建这个实例；
- 三是它必须自行向整个系统提供这个实例。

在Python中，单例模式有以下几种实现方式。

* 方法一、实现__new__方法，然后将类的一个实例绑定到类变量_instance上；如果cls._instance为None，则说明该类还没有被实例化过，new一个该类的实例，并返回；如果cls._instance不为None，直接返回_instance，代码如下：

```python
class Singleton(object):
 
    def __new__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            orig = super(Singleton, cls)
            cls._instance = orig.__new__(cls, *args, **kwargs)
        return cls._instance
 
class MyClass(Singleton):
    a = 1
 
one = MyClass()
two = MyClass()
 
#one和two完全相同,可以用id(), ==, is检测
print id(one)    # 29097904
print id(two)    # 29097904
print one == two    # True
print one is two    # True
```

* 方法二、本质上是方法一的升级版，使用__metaclass__（元类）的高级python用法，具体代码如下

```python
class Singleton2(type):
 
    def __init__(cls, name, bases, dict):
        super(Singleton2, cls).__init__(name, bases, dict)
        cls._instance = None
 
    def __call__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super(Singleton2, cls).__call__(*args, **kwargs)
        return cls._instance
 
class MyClass2(object):
    __metaclass__ = Singleton2
    a = 1
 
one = MyClass2()
two = MyClass2()
 
print id(one)    # 31495472
print id(two)    # 31495472
print one == two    # True
print one is two    # True
```

使用元类(__metaclass__)和可调用对象(__call__)
Python的对象系统中一些皆对象,类也不例外,可以称之为”类型对象”,比较绕,但仔细思考也不难:类本身也是一种对象,只不过这种对象很特殊,它表示某一种类型。是对象,那必然是实例化来的,那么谁实例化后是这种类型对象呢?也就是元类。
Python中,class关键字表示定义一个类对象,此时解释器会按一定规则寻找__metaclass__,如果找到了,就调用对应的元类实现来实例化该类对象;没找到,就会调用type元类来实例化该类对象。
__call__是Python的魔术方法,Python的面向对象是”Duck type”的,意味着对象的行为可以通过实现协议来实现,可以看作是一种特殊的接口形式。某个类实现了__call__方法意味着该类的对象是可调用的,可以想像函数调用的样子。再考虑一下foo=Foo()这种实例化的形式,是不是很像啊。结合元类的概念,可以看出,Foo类是单例的,则在调用Foo()的时候每次都返回了同样的对象。而Foo作为一个类对象是单例的,意味着它的类(即生成它的元类)是实现了__call__方法的。所以可以如下实现:

```python
class Singleton(type):
    def __init__(cls, name, bases, attrs):
        super(Singleton, cls).__init__(name, bases, attrs)
        cls._instance = None
    def __call__(cls, *args, **kwargs):
        if cls._instance is None
            # 以下不要使用'cls._instance = cls(*args, **kwargs)', 防止死循环,
            # cls的调用行为已经被当前'__call__'协议拦截了
            # 使用super(Singleton, cls).__call__来生成cls的实例
            cls._instance = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instance

class Foo(object): #单例类
    __metaclass__ = Singleton

>>>a = Foo()
>>>b = Foo()
>>>a is b
>>>True
>>>a.x = 1
>>>b.x
>>>1
```

* 方法三、使用Python的装饰器(decorator)实现单例模式，这是一种更Pythonic的方法；单利类本身的代码不是单例的，通装饰器使其单例化，代码如下：

```python
def singleton(cls, *args, **kwargs):
    instances = {}
    def _singleton():
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return _singleton
 
@singleton
class MyClass3(object):
    a = 1
 
one = MyClass3()
two = MyClass3()
 
print id(one)    # 29660784
print id(two)    # 29660784
print one == two    # True
print one is two    # True
```

使用类装饰器
使用装饰器实现单例类的时候,类本身并不知道自己是单例的,所以写代码的人可以不care这个,只要正常写自己的类的实现就可以,类的单例有装饰器保证。

```python
def singleton(cls):
    instances = {}
    def _wrapper(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return _wrapper
```

你会发现singleton装饰器内部使用了一个dict。当然你也可以用其他的方式,不过以下的实现是错误的:

```python
def singleton(cls):
    _instance = None #外部作用域的引用对于嵌套的内部作用域是只读的
    def _wrapper(*args, **kwargs):
        if _instance is None: #解释器会抛出"UnboundLocalError: ...referenced before assignment"
            _instance = cls(*args, **kwargs) #赋值行为使解释器将"_instance"看作局部变量
        return _instance
    return _wrapper 
```

所有资源资源调用者都是同一个对象，我首先想到的就是装饰器，可以很方便的 给不同的对象增添相同的功能。
Python 官方 wiki 给出了一个非常优雅的实现：

```python
def singleton(cls):
    instance = cls()
    instance.__call__ = lambda: instance
    return instance

# Sample use

@singleton
class Highlander:
    x = 100
    # Of course you can have any attributes or methods you like.


highlander = Highlander()
another_highlander = Highlander()
assert id(highlander) == id(another_highlander)
```

上面的代码定义了一个 singleton 装饰器，覆盖了类的 __call__ 方法， 该方法会在类创建的时候创建一个类的实例，并在之后类每次的实例化时总是返回这个实例对象。

当然，如果你希望只在需要的时候创建类的实例对象也有别的方法：

```python
def singleton(cls, *args, **kw):
    instances = {}
    def _singleton():
        if cls not in instances:
            instances[cls] = cls(*args, **kw)
        return instances[cls]
    return _singleton

@singleton
class MyClass(object):
    a = 1
    def __init__(self, x=0):
        self.x = x

one = MyClass()
two = MyClass()

assert id(one) == id(two)
```

上面的代码中实现了这样一个装饰器：装饰器函数创建的时候会创建一个 instances 字典， 该字典用于保存被装饰器修改过的类的实例，在类初始化的时候首先判断是否存在其实例， 如果存在则直接返回，否则创建一个新的实例保存到 instances 字典中，并返回该实例。 （这段代码出自 [cnblogs](http://www.cnblogs.com/goodhacker/p/3366618.html)）

## 重载__new__方法

```python
class Singleton(object):
    def __new__(cls, *args, **kwargs):
        ''' A pythonic singleton '''
        if '_inst' not in vars(cls):
            cls._inst = super().__new__(cls, *args, **kwargs)
        return cls._inst
    def __init__(self, *args, **kwargs):
        pass
```

使用基类
__new__是真正创建实例对象的方法，所以重写基类的__new__方法，以此来保证创建对象的时候只生成一个实例

```python
class Singleton(object):
    def __new__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            cls._instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
        return cls._instance


class Foo(Singleton):
    pass

foo1 = Foo()
foo2 = Foo()

print foo1 is foo2  # True
```
__init__不是Python对象的构造方法,__init__只负责初始化实例对象,在调用__init__方法之前,会首先调用__new__方法生成对象,可以认为__new__方法充当了构造方法的角色。所以可以在__new__中加以控制,使得某个类只生成唯一对象。具体实现时可以实现一个父类,重载__new__方法,单例类只需要继承这个父类就好。


```python
class Singleton(object):
    def __new__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            cls._instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
        return cls._instance

class Foo(Singleton): #单例类
    a = 1
```

## 使用装饰器

* 第一种实现方法

```python
class SingletonDecorator(object):
    def __init__(self, cls):
        self._cls = cls
        self._inst = None
    def __call__(self, *args, **kwargs):
        ''' Over __call__ method. So the instance of this class
        can be called as a function. '''
        if not self._inst:
            self._inst = self._cls(*args, **kwargs)
        return self._inst

class DemoCls(object):
    pass

DemoCls = SingletonDecorator(DemoCls)
# After this the DemoCls is bind with a SingletonDecorator instance
a = DemoCls()
b = DemoCls()
```

* 第二种实现方法
装饰器维护一个字典对象instances，缓存了所有单例类，只要单例不存在则创建，已经存在直接返回该实例对象。

```python
def singleton(cls):
    instances = {}

    def wrapper(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]

    return wrapper


@singleton
class Foo(object):
    pass

foo1 = Foo()
foo2 = Foo()

print foo1 is foo2
```

## 使用元类
元类是用于创建类对象的类，类对象创建实例对象时一定会调用__call__方法，因此在调用__call__时候保证始终只创建一个实例即可，type是python中的一个元类。

```python
class Singleton(type):
    def __call__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            cls._instance = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instance


class Foo(object):
    __metaclass__ = Singleton


foo1 = Foo()
foo2 = Foo()

print foo1 is foo2  # True
```

我自己对于不是很喜欢装饰器的实现，因为从语言逻辑上来看，我需要的是一个 有单例特性的类而不是为类添加单例限制。于是我又找到了基于 metaclass 的实现：

```python
class Singleton(type):
    def __init__(cls, name, bases, dict):
        super(Singleton, cls).__init__(name, bases, dict)
        cls._instance = None
    def __call__(cls, *args, **kw):
        if cls._instance is None:
            cls._instance = super(Singleton, cls).__call__(*args, **kw)
        return cls._instance

class MyClass(object):
    __metaclass__ = Singleton

one = MyClass()
two = MyClass()
```

上面的代码在类的第一次实例之后将这个个实例作为其类变量保存，在之后调用类的构造函数的时候 都直接返回这个实例对象。

这个解决方案强化了类与其单例之间的内聚性。