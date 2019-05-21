# Python基础

(参考Python Cookbook和Fluent Python)

(参考 https://www.liaoxuefeng.com/)

(参考菜鸟教程https://www.runoob.com/python3/)

---
---
### Python常识

#### Python中的基本数据类型？

Python3 中有六个标准的数据类型：

    Number（数字）
    String（字符串）
    List（列表）
    Tuple（元组）
    Set（集合）
    Dictionary（字典）

Python3 的六个标准数据类型中：

    不可变数据（3 个）：Number（数字）、String（字符串）、Tuple（元组）；
    可变数据（3 个）：List（列表）、Dictionary（字典）、Set（集合）。 

#### 如何理解Python 中的深浅拷贝？
参考 https://www.runoob.com/w3cnote/python-understanding-dict-copy-shallow-or-deep.html

```python
import copy

"""
注意：以下所说的都是可变数据类型，不能是数字和字符串。

对于直接赋值，只引用了最外层对象的地址，所以，原始对象无论如何改动，都会跟着变动；
"""
"""
对于浅拷贝，只复制第一层的地址值,所以子对象（例如子列表）变化时，会跟随着一起变化；
"""
"""
而对于深拷贝，会复制内部所有的嵌套地址，所以已经完全独立出来了，不会随原对象变化而变化；
"""

a = [1, 2, 3, 4, ['a', 'b']]  # 原始对象

b = a  # 赋值，传对象的引用
c = copy.copy(a)  # 对象拷贝，浅拷贝
d = copy.deepcopy(a)  # 对象拷贝，深拷贝

a.append(5)  # 修改对象a
a[4].append('c')  # 修改对象a中的['a', 'b']数组对象

print('原始列表      a = ', a)
print('直接赋值引用的 b = ', b)
print('浅拷贝        c = ', c)
print('深拷贝        d = ', d)

"""
打印结果是：
原始列表      a =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
直接赋值引用的 b =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
浅拷贝        c =  [1, 2, 3, 4, ['a', 'b', 'c']]
深拷贝        d =  [1, 2, 3, 4, ['a', 'b']]
"""

```

#### 如何理解Python中的迭代器和生成器？

1. 迭代器（iterator）是一个可以记住遍历的位置的对象。迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

    迭代器有两个基本的方法：iter() 和 next()， iter()用于创建迭代器对象， next()用于输出迭代器的下一个元素。字符串，列表或元组对象都可用于创建迭代器。
    
    创建生成器的常见方法有两个：
    
    1）用Iter()转换可迭代对象；
    
    2）在类中实现两个方法 __iter__() 与 __next__()；

    
```python

class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration
 
myclass = MyNumbers()
myiter = iter(myclass)
 
for x in myiter:
  print(x)

```

2.  在 Python 中，使用了 yield 的函数被称为生成器（generator）。

    或者使用生成器表达式 如：(i for i in range(10))

    跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

    在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。调用一个生成器函数，返回的是一个迭代器对象。
    
```python
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
```
    

#### 传递参数中的 *args and **kwargs

 *接受到的参数转成元组（意味着不限数量）

 ** 接受到的参数转成字典

只是约定，并非强制格式，但是 *args 必须在 **kwargs之前。


#### Python中的装饰器？

例如：
```python
import time

def metric(fn):
    def wrapper(*args):
        start = time.time()
        res = fn(*args)
        end = time.time()
        print('%s executed in %s s' % (fn.__name__, round(end - start, 3)))
        return res

    return wrapper


def logging(fn):
    def wrapper(*args, **kwgs):
        start = time.time()
        res = fn(*args, **kwgs)
        end = time.time()
        with open("log.txt", 'a') as f:
            line = "excute in %f s   @   " % (end - start) + time.strftime('%Y-%m-%d, %H:%M:%S\n', time.localtime(time.time()))
            f.write(line)
        print("time using:", end - start)
        return res
    return wrapper

```

---
---
### Python面向对象

#### Python中变量使用单下划线和双下划线的区别？

理解上参考Java签名中的作用域限定。

"单下划线" 开始的成员变量叫做保护变量，意思是只有类对象和子类对象自己能访问到这些变量； 以单下划线开头的变量和函数被默认是内部函数，使用from module import *时不会被获取，但是使用import module可以获取。

"双下划线" 开始的是私有成员，意思是只有类对象自己能访问，连子类对象也不能访问到这个数据。

此外，以单下划线结尾仅仅是为了区别该名称与关键词，前后双下划线是Python语言自用的魔方方法。


#### Python中的元类？
参考：https://www.liaoxuefeng.com/wiki/897692888725344/923030550637312

和： https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python

当我们定义了类以后，就可以根据这个类创建出实例，所以是先定义类，然后创建实例。

但是如果我们想创建出类呢？那就必须根据metaclass创建出类，所以是先定义metaclass，然后创建类。

连接起来就是：先定义metaclass，就可以创建类，最后创建实例。

所以，metaclass允许你创建类或者修改类。换句话说，你可以把类看成是metaclass创建出来的“实例”。



#### 什么是Python的自省(Introspection)? 一般用来做什么？

自省：自省就是能够获得自身的结构和方法，给开发者可以灵活的调用，给定一个对象，返回该对象的所有属性和函数列表，或给定对象和该对象的函数或者属性的名字，返回对象的函数或者属性实例。
常用的自省函数有：type(),dir(),getattr(),hasattr(),setattr(), delattr(), isinstance().

其中：isinstance 和 type 的区别在于：
 
    type()不会认为子类是一种父类类型。
    isinstance()会认为子类是一种父类类型。

看到一个说法：“自省”应该是语言原本的概念，特指在运行时获得object自身信息，是一种能力；“反射”是自省的一种实现方式，是具体的。

结合Java的反射来理解，但是又有点区别，留坑待补，之后详细学习一下。

#### 什么是Python的协程？ 

