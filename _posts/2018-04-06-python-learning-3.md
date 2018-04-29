---
layout: post
key: 20180406
tags: Python set file decode encode function
---

<!--more-->

#### 集合`set`

> 特点：无序，去重，关系测试
>
> 创建集合：
>
> ```python
> #method 1
> list_1 = [1,2,3,4,5,3,6,7,9,8]
> set_1 = set(list_1) #>>>{1, 2, 3, 4, 5, 6, 7, 8, 9}
> #method 2
> set_2 = {2, 4, 6, 8, 88}
> ```
>
> 关系测试：
>
> ```python
> ###交集
> set_1.intersection(set_2) #>>>{8, 2, 4, 6}
> set_1 & set_2 #method 2
>
> ###并集
> set_2.union(set_1) #>>>{1, 2, 3, 4, 5, 6, 7, 8, 9, 88}
> set_1 | set_1 #method 2
>
> ###差集
> set_1.difference(set_2) #>>>{1, 3, 5, 7, 9}
> set_1 - set_2 #method 2
> set_2.difference(set_1) #>>>{88}
> set_2 - set_1 #method 2
>
> ###对称差集
> #两个集合的并集减去两个集合的交集
> set_1.symmetric_difference(set_2) #>>>{1, 3, 5, 7, 9, 88}
> set_1 ^ set_2 #method 2
>
> ###子集
> set_2.issubset(set_1) #>>>False
> set_2.issuperset(set_1) #>>>False
>
> #两个集合没有交集返回真，否则假
> set_1.isdisjoint(set_2) #>>>False
> ```
>
> 基本操作：
>
> ```python
> ###添加
> set_1.add(56) #添加一项
> set_1.update([10,11,'12']) #添加多项
>
> ###删除
> set_1.remove('12') #若存在则删除，否则报错
> set_1.pop() #随机，删除并返回这个元素
> set_1.discard('12') #删除指定数，若不存在返回None
>
> ###长度
> len(set_1)
>
> ###测试12是否是set_1成员，返回bool值
> 12 in set_1 #>>>False
>
> ###浅复制
> set_3 = set_1.copy()
> ```

#### 文件操作

> 源文件的中间修改只会覆盖重写，而不是插入！
>
> 打开文件：
>
> ```python
> ##f为文件句柄，光标指向文件开始
> #mode='r'：只读
> #mode='w'：以写方式打开文件，若文件里面有内容会被覆盖；文件不存在则创建一个
> #mode='a'：追加方式写文件
> #mode='r+'：读写(可读，追加写)--->推荐
> #mode='w+'：写读（覆盖内容，文件不存在则创建一个，追加写）
> #mode='a+'：追加读
> #mode='rb'：二进制格式读文件，没有encoding参数
> #mode='wb'：写二进制文件
> f = open("filename", mode = 'r', encoding='utf-8') 
>
> ###mode='r'
> data1 = f.read() #读完后，光标指向文件结束
> data2 = f.read() #因为光标已经指向文件结束的位置，故data2中没有数据
>
> ###mode='w'或mode='a'
> f.write("...")
> ```
>
> 读写行：
>
> ```python
> f = open("filename", mode='r',encoding='utf-8')
>
> ##行读文件，只适合小文件读写
> f.readline() #换行符\n会被读到
> f.readlines() #返回列表，文件的每一行作为列表的一个元素
> 			  #当然包括换行符\n(可用script()去除两端的空格和换行)
>
> ##行读文件，适合大文件
> #内存中只保存一行
> for line in f:
>     print(line) #对line进行的操作
> ```
>
> 其他操作：
>
> ```python
> f = open("filename", mode='r',encoding='utf-8')
>
> data = f.read(int_n) #指定读int_n个字符
>
> f.tell() #返回文件句柄的光标现在所在的位置，按字符个数来计数
>
> f.seek(int_n) #移动文件句柄的光标位置至int_n位置
>
> f.encoding #返回文件编码方式
> f.name #返回文件名字
> f,closed #判断文件是否已经关闭
>
> f.fileno() #文件在系统中的编号
>
> f.seekable() #判断文件是否可以移动文件句柄的光标
> f.readable() #判断文件是否可读
> f.writable() #判断文件是否可写
>
> f.flush() #强制刷新。文件先写到缓存中，使用这个方法后，不等缓存写满就直接写入文件中
>
> ###stdout
> #有间隔的输出
> import sys,time
> for i in range(20):
>     sys.stdout.write("#")
>     sys.stdout.flush()
>     time.sleep(0.5)
>     
> #截断
> f.truncate(int_n) #只保留文件开始至int_n位置的字符，其余清除，无论光标在何处
> ```
>
> 修改文件：
>
> ```python
> ###method 1
> #vim文件编辑器，但是需要等待整个文件加载至内存中
>
> ###method 2
> #先读文件1，当读到相应的行后，使用字符串的replace()方法，最后存文件
> f_read = open('read_file', 'r', encoding='utf-8')
> f_write = open('write_file', 'w', encoding='utf-8')
> find_str = sys.argv[1] #要找到的字符串，由输入得到
> replace_str = sys.argv[2] #要修改的字符串，由输入得到
> for line in f_read:
>     if find_str in line:
>         line = line.replace(find_str, replace_str)
>     f_write.write(line)
> f_read.close()
> f_write.close()
> ```
>
> 关闭文件：
>
> ```python
> ###method 1
> f.close()
>
> ###method 2
> #with自动关闭文件，无需写f.close()
> with open('file1_name', 'r') as f1, open('file2_name', 'w') as f2:
>     ... #对文件的操作
> ```

#### 字符编码与转码

>编码：
>
>> `ASCII`编码：英文字符占一个字节，不存中文
>>
>> `Unicode`编码：所有的字符都占两个字节
>>
>> `utf-8`编码：英文占一个字节，中文占三个字节；是`Unicode`的扩展集，可以直接打印
>
>```python
>import sys
>sys.getdefaultencoding() #系统的默认编码方式
>```
>
>转换（以Unicode为中介）：一种编码方式 <------> `Unicode` <------>另一种编码方式
>
>```python
>gbk_to_utf8.decode('gbk').encode('utf-8') #gbk先解码成Unicode编码，Unicode再编码成utf-8
>utf8_to_gbk.decode('utf-8').encode('gbk') #utf-8先解码成Unicode编码，Unicode再编码成gbk
>```
>
>文件编码和Python编码：
>
>```python
>###文件编码(存在硬盘上的)
>#-*-coding:gbk-*- #强调文件编码方式为gbk
>
>###python编码默认为Unicode
>#将Unicode编码后转成二进制了
>'nihao你好'.encode('gbk') #>>>b'nihao\xc4\xe3\xba\xc3'
>```

#### 函数

> 特点：参数+文档+返回值
>
> > - 代码重复利用
> > - 可扩展
> > - 保持一致性
>
> 返回值：
>
> > - 返回值数=0：返回`None`
> > - 返回指数=1：返回`object`
> > - 返回指数>1：返回`tuple`
>
> 函数调用的参数：
>
> > - **位置参数**和**关键参数**：关键参数调用不能写在位置参数前面`test(3,z=2,y=4)`
> >
> > - **默认参数**：
> >
> > - **参数组**：当实参个数不固定时，将形参定义为`*args`
> >
> >   ```python
> >   ###把多个不固定的位置实参转换成元组的方式传入形参
> >   def test(*args):
> >       print(args)
> >   test(1,2,3,4,5) #>>>(1,2,3,4,5)
> >   test([1,2,3,4,5]) #>>>([1,2,3,4,5],)
> >   test(*[1,2,3,4]) #>>>(1,2,3,4)
> >
> >   ###把多个不固定的关键字实参转换成字典的方式传入形参
> >   def test_dict(**kwargs):
> >       print(kwargs)
> >   test_dict(name='A',age=2,sex='N') #>>>{'name':'A','age':2,'sex':'N'}
> >   test_dict(**{name='A',age=2,sex='N'}) #>>>{'name':'A','age':2,'sex':'N'}
> >
> >   ###组合
> >   def test_1(name,age=12,**kwargs):
> >       print(name,age,kwargs)
> >   test_2('A',age=2,sex='N') #>>>A2{sex':'N'}
> >   test_2('A',2,sex='N') #>>>A2{sex':'N'}
> >   test_2('A',sex='N',age=3) #>>>A3{sex':'N'}
> >   test_2('A',2,sex='N',age=3) #出错，age有两个实参传入
> >
> >   def test_3(name,age=12,*args,**kwargs):
> >       print(name,age,args,kwargs)
> >   test_3('A',age=2,sex='N') #>>>A2(){sex':'N'}
> >   ```
>
> 局部变量：
>
> > 整数和字符串在局部修改后不影响全局，而复杂的数据结构在局部修改后会影响全局
> >
> > ```python
> > global var_name #若没有全局变量则创建一个，有则使用全局变量---->不推荐
> > ```
>
> 递归：最大递归深度999次
>
> ```python
> def calc(n):
>     print(n)
>     if int(n/2) > 0:
>         return calc(int(n/2))
>     print("-->",n)
> calc(10) #10 5 2 1 -->1
> ```
>
> 函数式编程：`lisp`、`hashshell`、`erlang`
>
> 高阶函数：一个函数的参数可以是函数
>
> ```python
> def add(a,b,f):
>     return f(a)+f(b)
> res = add(3,-6,abs) #>>>9
> ```

#### 过程

> 特点：没有返回值的函数，Python中隐式赋予其返回值为`None`