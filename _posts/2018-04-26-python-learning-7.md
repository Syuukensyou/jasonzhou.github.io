---
layout: post
keys: 20180426
tags: Python OOP Class Object
modify_date: 2018-04-28
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
> 
>
> 实现：
>
> >`__init__(self,...)`：
> >
> >> - 名称：构造函数
> >> - 功能：要实现一个类的实例只能通过此函数来传递参数
> >
> >
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
> >> ![class](https://github.com/Syuukensyou/syuukensyou.github.io/tree/master/_posts_pic/class.png)
> >
> >
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
> >
> >
> >析构函数：
> >
> >> 功能：在实例释放、销毁的时候**自动执行**，如关闭一些数据库的连接etc
> >>
> >> `__del__(self)`：`del r1`时直接调用析构函数
> >
> >
> >
> >私有方法、私有属性：
> >
> >>名称前有两个短下划线：`__attr`
> >>
> >>可以在内部访问，但不可在外部直接访问
> >
>
> 
>
> 继承：
>
> >功能：在子类中可以重写父类方法；也可以先调用父类方法再补充自己的方法；代码重用
> >
> >
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
> >
> >
> >子类实例化顺序：
> >
> >> 先执行子类的构造函数A，若没有则直接去找继承的第一个父类的继承函数B，若没有则去找继承的第二个父类的继承函数C......**直到找到一个父类有构造方法`(__init__())`的类，就不再往后面继续寻找**
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
> >
> >多重继承：
> >
> >> `Python2`：
> >>
> >> > 经典类继承顺序：深度优先
> >> >
> >> > 新式类继承顺序：广度优先
> >>
> >> `Python3`：
> >>
> >> > 经典类、新式类继承顺序：广度优先！
> >
> >
> >
> >实例：
> >
> >```python
> >class School(object):
> >    """学校类"""
> >    def __init__(self,name,addr):
> >        self.name = name
> >        self.addr = addr
> >        self.students =[]
> >        self.staffs =[]
> >        
> >    def enroll(self,stu_obj):
> >        print("为学员%s 办理注册手续" % stu_obj.name )
> >        self.students.append(stu_obj)
> >        
> >    def hire(self,staff_obj):
> >        self.staffs.append(staff_obj)
> >        print("雇佣新员工%s" % staff_obj.name)
> >
> >class SchoolMember(object):
> >    """学校成员类，目前可以派生学生和教师"""
> >    def __init__(self,name,age,sex):
> >        self.name = name
> >        self.age = age
> >        self.sex = sex
> >        
> >    def tell(self):
> >        """打印成员信息，在子类中实现"""
> >        pass
> >
> >class Teacher(SchoolMember):
> >    """教师类"""
> >    def __init__(self,name,age,sex,salary,course):
> >        super(Teacher,self).__init__(name,age,sex)
> >        self.salary = salary
> >        self.course = course
> >        
> >    def tell(self):
> >        """在子类中重写父类的打印方法"""
> >        print('''
> >        ---- info of Teacher:%s ----
> >        Name:%s
> >        Age:%s
> >        Sex:%s
> >        Salary:%s
> >        Course:%s
> >        ''' % (self.name,self.name,self.age,self.sex,self.salary,self.course))
> >
> >    def teach(self):
> >        print("%s is teaching course [%s]" % (self.name,self.course))
> >
> >class Student(SchoolMember):
> >    """学生类"""
> >    def __init__(self,name,age,sex,stu_id,grade):
> >        super(Student,self).__init__(name,age,sex)
> >        self.stu_id = stu_id
> >        self.grade = grade
> >        
> >    def tell(self):
> >        print('''
> >        ---- info of Student:%s ----
> >        Name:%s
> >        Age:%s
> >        Sex:%s
> >        Stu_id:%s
> >        Grade:%s
> >        ''' % (self.name, self.name, self.age, self.sex, self.stu_id, self.grade))
> >        
> >    def pay_tuition(self,amount):
> >        print("%s has paid tution for $%s" % (self.name,amount) )
> >
> >
> >school = School("A","a")
> >
> >t1 = Teacher("A1",56,"MF",200000,"Linux")
> >t2 = Teacher("A2",22,"M",3000,"PythonDevOps")
> >
> >s1 = Student("B1",36,"MF",1001,"PythonDevOps")
> >s2 = Student("B2",19,"M",1002,"Linux")
> >
> >t1.tell()
> >s1.tell()
> >school.hire(t1)
> >school.enroll(s1)
> >school.enroll(s2)
> >
> >print(school.students)
> >print(school.staffs)
> >school.staffs[0].teach()
> >
> >for stu in school.students:
> >    stu.pay_tuition(5000)
> >```
>
> 
>
> 多态：
>
> >作用：接口重用。同一接口，多种实现
> >
> >实例：
> >
> >```python
> >class Animal:
> >    def __init__(self, name):
> >        self.name = name
> >
> >    def talk(self):
> >        pass
> >    
> >    @staticmethod
> >    def animal_talk(obj):
> >        obj.talk()
> >
> >class Cat(Animal):
> >    def talk(self):
> >        print('Meow!')
> >
> >class Dog(Animal):
> >    def talk(self):
> >        print('Woof! Woof!')
> >
> >d = Dog("A")
> >#d.talk()
> >
> >c = Cat("B")
> >#c.talk()
> >
> >Animal.animal_talk(c)
> >Animal.animal_talk(d)
> >```
>
> 
>
> 类的方法：
>
> >###方法的下划线区别：
> >
> >**两头下划线**：Python类内置成员专用，区别用户自定义成员
> >
> >**单下划线**：类的普通成员
> >
> >**双下划线**：解析器自动转换为：`_类名__成员名`代替原有成员，访问需要在原有成员名字前加上_类名。
> >
> >
> >
> >**静态方法**:`@staticmethod`
> >
> >> 名义上归类管，就是正常的函数，实际上不能访问类或实例的任何属性；
> >>
> >> 需要通过类名来调用，参数不默认有`self`；
> >
> >**类方法**:`@classmethod`
> >
> >>只能访问类变量，不能访问实例变量；
> >
> >**属性方法**：`@property`
> >
> >>把一个方法变成一个静态属性；隐藏实现细节
> >>
> >>**不能传参数，不能赋值！**
> >>
> >>**正常属性是可以删除的，但是属性方法不能删除！**
> >>
> >>**注意：**想传参数，想赋值，想删除
> >>
> >>```python
> >>class Dog(object):
> >>    def __init__(self, name, __food):
> >>        self.name = name
> >>        self.__food = None
> >>        
> >>    @property
> >>    def eat(self):
> >>        print("%s is eating %s" % (self.name, self.__food))
> >>        
> >>    @eat.setter  # 修改
> >>    def eat(self, food):
> >>        self.__food = food
> >>        print("set to food:", food)
> >>        
> >>    @eat.deleter  # 删除
> >>    def eat(self):
> >>        del eat.__food
> >>        print("delete the eat")
> >>        
> >>d = Dog("A")
> >>d.eat = "baozi"  # 调用属性方法的setter，直接赋值
> >>d.eat  # 直接调用属性方法，因为不能通过函数调用方式
> >>del d.eat  # 删除属性方法
> >>```
> >>
> >>实例：航班查询
> >>
> >>```python
> >>class Flight(object):
> >>    def __init__(self,name):
> >>        self.flight_name = name
> >>
> >>    def checking_status(self):
> >>        print("checking flight %s status " % self.flight_name)
> >>        return  0
> >>
> >>    @property
> >>    def flight_status(self):
> >>        status = self.checking_status()
> >>        if status == 0 :
> >>            print("flight got canceled...")
> >>        elif status == 1 :
> >>            print("flight is arrived...")
> >>        elif status == 2:
> >>            print("flight has departured already...")
> >>        else:
> >>            print("cannot confirm the flight status...,please check later")
> >>    @flight_status.setter
> >>    def flight_status(self,status):
> >>        print("flight %s has changed status to %s" %(self.flight_name,status))
> >>f = Flight("CA980")
> >>f.flight_status  # 对用户隐藏实现细节
> >>f.flight_status = 2
> >>```
> >
> >
> >
> >- `__doc__`：获取类的说明信息
> >
> >- `__module__`：表示当前操作的对象是从哪个模块导入的
> >
> >- `__class__`：当前类本身
> >
> >- `__init__`
> >
> >- `__del__`
> >
> >- `__call__`：由对象直接触发的方法
> >
> >- `__dict__`：通过类调用--->所有属性和方法，不包括实例属性；通过实力调用--->所有实例属性，不包括类属性
> >
> >- `__str__`：获得**实例**的消息，面向用户的，
> >
> >- `__repr__`：获取**类**的消息，面向程序员的
> >
> >  > 当`print(obj)`时，会优先使用`__str__`，如果其未重写，则再使用`__repr__`
> >
> >- `__getitem__`、`__setitem__`、`__delitem__`：字典，执行这个动作，动作内部的动作需要自己定义
> >
> >
> >```python
> >class Foo(object):
> >    def __init__(self):
> >        self.data = {}
> >        
> >    def __getitem__(self, key):
> >        print('__getitem__', key)
> >        return self.data.get(key)
> >    
> >    def __setitem__(self, key, value):
> >        print('__setitem__', key, value)
> >        self.data[key] =value
> >    
> >    def __delitem__(self, key):
> >        print('__delitem__', key)
> >
> >obj = Foo()
> >obj['name'] = "alex"
> >del obj["sdfdsf"]
> >```
> >
> >- `__new__`：通常用于控制生成一个新实例的过程。是**类级别**的方法。用来创建实例的方法，通过new来调用init，实例化之前定制，先执行`__new__`再执行`__init__`；**必须有返回值**
> >
> >  >作用：
> >  >
> >  >- 主要当继承一些不可变的`class`时（如`int`、`str`、`tuple`），提供给你一个自定义这些类的实例化过程的途径。
> >  >- 单例模式（`singleton`）
> >
> >- `__init__`：通常用于初始化一个新实例，控制这个初始化的过程，如添加一些属性，做一些额外的操作，**发生在类实例被创建完以后**。是**实例级别**的方法
> >
> >
> >```python
> >def func(self):
> >    print('hello %s' %self.name)
> >    
> >def __init__(self,name,age):
> >    self.name = name
> >    self.age = age
> >
> ># 封住一个类，特殊方法创建类
> >Foo = type('Foo',  # 类名
> >           (object,),  # 继承父类
> >           {'talk': func, '__init__':__init__}  # 包含的方法
> >          )
> >f = Foo("Chrn",22)
> >f.talk()
> >print(type(Foo))
> >```
> >
> >```python
> >class MyType(type):
> >    def __init__(self, what, bases=None, dict=None):
> >        print("--MyType init---")
> >        super(MyType, self).__init__(what, bases, dict)
> >
> >    def __call__(self, *args, **kwargs):
> >        print("--MyType call---")
> >        obj = self.__new__(self, *args, **kwargs)
> >        obj.data = {"name":111}
> >        self.__init__(obj, *args, **kwargs)
> >        
> >class Foo(object):
> >    def __init__(self, name):
> >        self.name = name
> >        print("Foo ---init__")
> >
> >    def __new__(cls, *args, **kwargs):
> >        print("Foo --new--")
> >        return object.__new__(cls) #继承父亲的__new__方法，cls相当于Foo
> >```
> >
> >![metaclass](https://github.com/Syuukensyou/syuukensyou.github.io/tree/master/_posts_pic/metaclass.png)
> >
> >`http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python`
> >
> >
> >
> >- `getattr(object, name)`、`hasattr(object, name)`、`setattr(object, name)`、`delattr(object, name)`：查找实例object中是否有对应name字符串的方法、属性
> >
> >```python
> >def bulk(self):
> >    print("%s is yelling...." % self.name)
> >
> >class Dog(object):
> >    def __init__(self,name):
> >        self.name = name
> >
> >    def eat(self,food):
> >        print("%s is eating ... " % self.name,food)
> >
> >d = Dog("A")
> >choice = input(">>:").strip()
> >
> >if hasattr(d,choice):  # 判断对象中是否有对应字符串的方法或属性
> >    func = getattr(d,choice)  # 获取该方法至func中，便于传参
> >    func("baozi")  # 通过func方法可以传入参数
> >    #  equals：
> >    #  getattr(d, choice)("baozi")
> >else:
> >    # 如果添加的是一个方法则可以调用，如果是一个属性则可直接获取
> >    setattr(d,choice,bulk)  # d.talk = bulk(传入的choice是talk)
> >    func = getattr(d, choice)
> >    func(d)
> >```