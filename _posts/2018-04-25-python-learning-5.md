---
layout: post
key:20180425
tags: generator Iterable Iterator
---

列表生成器：

>```python
>a = [i*2 for i in range(10)]
>### 相当于：
>a = []
>for i in range(10):
>    a.append(i*2)
>```
>

生成器：

> 特点：
>
> > 节省内存，避免因开辟所有数据占用非常多的空间；
> >
> > 不立可生成数据，只有当使用时才生成数据，不能以下标访问；
> >
> > 只记住当前位置，不记得前面的位置和后面的位置！
> >
> > `yield`：保存当前位置，暂停后面的继续运行；
> >
> > `.__next__()`：调用`yield`但不会传值
> >
> > `.send(...)`：调用`yield`且会把值传入
>
> ```python
> b = (i*2 for i in range(10)) # 生成器
>
> ### 直接访问生成器会报错
> # err
> b[5] # TypeError
>
> ### 取数
> # 只取当前
> b.__next__()
> ```
>
> 实例-----斐波那契
>
> ```python
> def fib(max):
>     n, a, b = 0, 0, 1
>     while n<max:
>         yield b # 实现一个生成器，保存当前函数的状态
>         a, b = b, a+b # 赋值语句，其中赋值号的右边是一个元祖（因为逗号）
>         n = n+1
>     return '*-*-*-done-*-*-*' # 函数异常时会返回的异常值
> g = fib(10)
> ### 捕获异常
> while True:
>     try:
>         x = g.__next__()  # equals x=next(g)
>         print(x)
>     except StopIteration as ST:
>         print('Generator retuan value:', ST.value)
>         break   
> ```
>
> 异步IO的雏形（**协程**）：
>
> ```python
> import time
> def consumer(name):
>     print("%s 准备吃包子啦!" %name)
>     while True:
>        baozi = yield
>        print("包子[%s]来了,被[%s]吃了!" %(baozi,name))
> def producer(name):
>     c = consumer('A')  # 将函数变为生成器
>     c2 = consumer('B')  # 将函数变为生成器
>     c.__next__()
>     c2.__next__()
>     print("开始准备做包子啦!")
>     for i in range(10):
>         time.sleep(1)
>         print("做了2个包子!")
>         c.send(i)  # 将i传给baozi
>         c2.send(i)
>
> producer("C")
> ```

迭代器：

> 可迭代对象：
>
> > I.集合数据类型：list, tuple, dict, set, str...
> >
> > II.`generator`，包括生成器和带`yield`的`generator  function`
> >
> > 
>
> ```python
> from collection import Iterable
> from collection import Iterator
> ```
>
> 可直接作用于for循环的对象--->**可迭代对象**（`Iterable`）
>
> 可以被`next()`函数调用并不断返回下一个值的对象--->**迭代器**（`Iterator`）【数据流，且不知道序列长度】
>
> 
>
> `isinstance()`来判断一个对象是否是可迭代对象、迭代器对象
>
> 
>
> 生成器都是Iterator对象，但`list`、`dict`、`str`等虽然是Iterable，却不是Iterator！
>
> 但是可以使用Iterator的内置函数`iter()`将`list`、`dict`、`str`等编程Iterator！

**小结：**

> 凡是可作用于`for`循环的对象都是`Iterable`类型；
>
> 凡是可作用于`next()`函数的对象都是`Iterator`类型，它们表示一个**惰性计算**的序列；
>
> 集合数据类型如`list`、`dict`、`str`等是`Iterable`但不是`Iterator`，但可通过`iter()`函数获得一个`Iterator`对象。
>
> Python的`for`循环本质上就是通过不断调用`next()`函数实现的，例如：
>
> ```python
> for x in [1,2,3,4,5]:
>     pass
> ### equals:
> it = iter([1,2,3,4,5])
> while True:
>     try:
>         x = next(it)
>     except StopIteration:
>         break
> ```



**`dir(object)`可以查看object的所有方法**