---
layout: post
key: 201804013
tags: Python decorator
---



装饰器：

> 定义：本质是函数（装饰其他函数），就是为其他函数添加附加功能
>
> 原则：
>
> > 1.不能修改被装饰的函数的源码
> >
> > 2.不能修改被装饰的函数的调用方式
> >
> > 3.装饰器对于被修饰的函数来说是透明的，即被修饰的函数感受不到装饰器的存在
>
> 实现装饰器的必要：（**高阶函数+嵌套函-->装饰器**）
>
> >1.函数即“变量”
> >
> > >```python
> > >>>###1
> > >>>#bar()未定义，会报错
> > >>>def foo():
> > >>>    print('in the foo')
> > >>>    bar()
> > >>>foo()
> > >>>
> > >>>###2
> > >>>#可以正常运行
> > >>>def bar():
> > >>>    print('in the bar')
> > >>>def foo():
> > >>>    print('in the foo')
> > >>>    bar()
> > >>>foo()
> > >>>
> > >>>###3
> > >>>#可以正常运行
> > >>>def foo():
> > >>>    print('in the foo')
> > >>>    bar()
> > >>>def bar():
> > >>>    print('in the bar')
> > >>>foo()
> > >>>
> > >>>###4
> > >>>#bar未定义，不能正常运行
> > >>>def foo():
> > >>>    print('in the foo')
> > >>>    bar()
> > >>>foo()
> > >>>def bar():
> > >>>    print('in the bar')
> > >>>```
> > >>>
> > >>>python生成变量时，先在内存中存放一个值，再将变量名指向这个值，当指向这个值的变量名都不在时，python的回收机制定期刷新，如果没有变量名指向内存中的地址，就会回收内存！
> > >>>
> > >>>对于函数来说，现在内存中存放一个函数体，再将函数名指向这个函数体，当函数名不用时，函数体所占的内存就会被回收。匿名函数也是一样。
> > >>>
> > >>>变量的定义和使用：
> > >>>
> > >>>> 1.定义变量
> > >>>>
> > >>>> 2.调用变量（调用之前声明就不会未定义出错）
> > >>```
> > >```
> >
> >2.高阶函数（满足条件之一即可）
> >
> >> a.把一个函数名当作实参传给另外一个函数--->可以做到不修改被装饰函数源代码的前提下添加功能
> >>
> >> ```python
> >> ###能够正常运行
> >> def bar():
> >>     print('in the bar')
> >> def test(func): # 将函数名作为形参
> >>     print(func) # 打印函数名所指向的内存地址
> >>     func() #运行函数名所指向内存地址的函数
> >> test(bar)
> >> ```
> >>
> >> b.返回之中包含函数名
> >>
> >> ```python
> >> ###能够正常运行
> >> def bar():
> >>     print('in the bar')
> >> def test(func): # 将函数名作为形参
> >>     print(func) # 打印函数名所指向的内存地址
> >>     return func #返回函数名所指向的内存地址
> >> bar = test(bar) #将返回的函数内存地址付给bar
> >> bar() #相当于调用所指向内存地址的函数
> >> ```
> >
> >3.嵌套函数
> >
> >> 在函数内部可以再定义函数
>
> 装饰器案例：
>
> >```python
> >###被装饰函数没有参数-->不通用！！！
> >import time
> >def timer(func):
> >    def deco():
> >        start_time = time.time()
> >        func()
> >        end_time = time.time()
> >        print('the func run time is %s' % (start_time-end_tiem))
> >        # NO return
> >    deco()
> >
> >@timer # 等同于foo=timer(foo)
> >def foo():
> >    print('in the foo')
> >foo() #此时指向foo()就是执行deco()，因为@timer做了偷梁换柱
> >```
> >
> >```python
> >###被装饰函数有参数，不知道几个，为了装饰器能够适应具有各个参数的情况，做以下改进-->通用！！！
> >import time
> >def timer(func):
> >    def deco(*args,**kwargs): # 这个参数的应用极致！
> >        start_time = time.time()
> >        func(*args,**kwargs) # 极致！
> >        end_time = time.time()
> >        print('the func run time is %s' % (start_time-end_tiem))
> >        # NO return
> >    deco()
> >
> >@timer # 等同于foo=timer(foo)
> >def foo(name):
> >    print('in the foo ',name)
> >foo('bug') #此时指向foo()就是执行deco()，因为@timer做了偷梁换柱
> >```
> >
> >```python
> >###终极版
> >import time
> >user,passwd = 'A','123456'
> >def auth(auth_type):
> >    print("auth type: ", auth_type)
> >    def deco(func):
> >        def wrapper(*args,**kwargs):
> >            if auth_type=='local':
> >                username = input('User name: ').strip()
> >                password = input('Password: ').strip()
> >                if username==user and password==passwd:
> >                    print('logging...')
> >                    return func(*args,**kwargs) # 参数必须给func，因为被装饰函数有返回值
> >                else:
> >                    print('No match user or passwd')
> >            elif auth_type=='remote':
> >                start_time = time.time()
> >                func(*args,**kwargs) # 因为被装饰函数无返回值，所以无需return
> >                end_time = time.time()
> >                print('the func run time is %s' % (end_time-start_time))
> >                print('No method to write this')
> >            else:
> >                exit('No match')
> >        return wrapper # must return！！！
> >    return deco # must return！！！
> >
> >def index():
> >    print('Welcome to index page!')
> >time.sleep(1.5)
> >@auth(auth_type='local') # 遇到装饰器先执行auth(),比index()等调用还有早
> >def home():
> >    print('Welcome to home page!')
> >    return 'home page return !'
> >time.sleep(1.5)
> >@auth(auth_type='remote') # 遇到装饰器先执行auth(),比index()等调用还有早
> >def bbs():
> >    print('Welcome to bbs page!')
> >
> >index()
> >home()
> >bbs()
> >```

