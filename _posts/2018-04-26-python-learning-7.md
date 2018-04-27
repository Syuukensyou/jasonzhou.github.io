---
layout: post
keys: 20180426
tags: Python OOP Class Object Object-Oriented-Programming Encapsulation Inheritance Polymorphism
---

OOP：

> 特性：
>
> > - Class类：一个类是对一类拥有相同的对象的抽象；
> >
> > - Object对象：一个对象是一个类的实例化后实例，一个类必须经过实例化后方可在程序中调用，一个类可以实例化多个对象，每个对象亦可有不同的属性；
> > - **Encapsulation-封装**：
> > - **Inheritance 继承**：
> > - **Polymorphism 多态**：“一个接口，多种实现”
>
> 实现：
>
> >`__init__(self,...)`：
> >
> >> - 名称：构造函数
> >> - 功能：要实现一个类的实例只能通过此函数来传递参数
> >
> >实现过程：
> >
> >> Step1：申请空间并命名
> >>
> >> Step2：把地址和赋值参数传给实例
> >>
> >> Step3：赋值
> >>
> >> ```python
> >> r1 = Role("A", "Police", "AK47", 1000)
> >>
> >> ### equals:
> >> Role(r1, "A", "Police", "AK47", 1000)
> >> # 即：
> >> r1.name = "A"
> >> r1.role = "Police"
> >> r1.weapon = "AK47"
> >> r1.money = 1000
> >>
> >> r1.prove = True  # 向实例r1中添加属性prove，仅限r1
> >> del r1.weapon  # 去除r1的属性weapon，仅限r1
> >> ```
> >
> >**注意**：
> >
> >> 只开辟**属性**的内存，**方法一直在类中**，而无需申请内存
> >>
> >> ```python
> >> r1.buy_gun("B22")
> >>
> >> ### equals:
> >> buy_gun(r1, "B22")
> >> ```
> >>
> >> **实例变量（静态属性）**：
> >>
> >> > - 作用域仅为实例本身；
> >> > - 可以通过实例修改实例变量，删除实例的实例变量；
> >>
> >> **类变量**：
> >>
> >> > - 存在类的内存中；
> >> > - 对象共有的属性！！！节省内存。。。
> >> > - 如果类变量和实例变量名称相同，则先查找实例变量再找类变量；
> >> > - 通过**实例**修改类变量其实是**为实例添加与类变量同名的实例变量** 或 **修改与类变量同名的实例变量**；
> >> > - 通过**类名**修改类变量则是真正意义上的修改，但不影响实例的(与类变量具有相同名称的)实例变量；
> >>
> >> **类的方法、功能（动态属性）**：
> >
> >析构函数：
> >
> >> 功能：在实例释放、销毁的时候**自动执行**，如关闭一些数据库的连接etc
> >>
> >> `__del__(self)`：`del r1`时直接调用析构函数
> >
> >私有方法、私有属性：
> >
> >>名称前有两个短下划线：`__attr`
> >>
> >>可以在内部访问，但不可在外部直接访问
> >
>
> 继承：
>
> >在子类中可以重写父类方法；也可以先调用父类方法再补充自己的方法
> >
> >重构构造函数：
> >
> >```python
> ># class People() 经典类
> >class People(object):  # 新式类
> >    def __init__(self, name, age):
> >        self.name = name
> >        self.age = age
> >    # TODO
> >class Man(People):
> >    def __init__(self, name, age, money):
> >        # 调用父类构造函数，注意所有参数
> >        # People.__init__(self, name, age)  # 经典类写法
> >        # another method--->推荐，当父类名称修改后，子类中就不需要修改
> >        super(Man, self).__init__(name, age)  # 新式类写法
> >        self.money = money
> >```
> >
> >子类实例化顺序：
> >
> >> 先执行子类的构造函数A，若没有则直接去找继承的第一个父类的继承函数B，若没有则去找继承的第二个父类的继承函数C......
> >
> >```python
> >class People(object):
> >    def __init__(self, name, age):
> >        self.name = name
> >        self.age = age
> >        self.friends = []
> >def Relation(object):
> >    def make_friends(self, obj):  # self在Man初始化就有name属性，同理obj也是
> >        print("{a} is making friends with {b}".format(a=self.name, b=obj.name))
> >        self.friends.append(obj)  # 必须是obj而不是obj.name，这样当实例变量修改名字后就obj的名字也随之改变
> >def Man(Relation, People):
> >    def __init__(self, name, age, money):
> >        super(Man,self).__init__(name, age)
> >        self.money = money
> >m1 = Man("A", 22)
> >m2 = Man("B", 23)
> >m1.make_friends(m2)
> >print(m1.friends[0])  # 应该是B
> >m2.name = "C"
> >print(m1.friends[0])  # 应该是C
> >```
> >
> >