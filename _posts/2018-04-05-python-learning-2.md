---
layout: post
key: 20180405
tags: Python list dict tuple str bytes
modify_date: 2018-04-06
---



#### 标准库：

> 1. `sys`
>
>    ```python
>    sys.path #Python使用的库存放在路径之一中，即环境变量
>
>    sys.avgr #打印脚本的相对路径
>    sys.avgr[i] #可以取位置（从0开始）为i的输入参数
>    ```
>
> 2. `os`（...为系统相关命令）
>
>    ```python
>    os.system("...") #执行系统命令，命令返回值为0即成功，不保存结果
>
>    os.popen("...") #执行系统命令，命令返回值为内存地址
>    os.popen("...").read() #读取内容，保存命令执行结果
>
>    os.mkdir("...") #创建新目录
>    ```



#### Python的运行过程：

> python程序运行时，编译结果保存在位于内存中的PyCodeObject中，当程序运行结束时，Python解释器则将PyCodeObject写回到pyc文件（预编译后的字节码文件）中。
>
> 当程序第二次运行时，首先程序会在硬盘中寻找pyc文件，若找到直接载入，否则从新编译解释。这里只需要比较一下pyc文件和源文件的更新时间就可以判断源文件是否改动过。



#### 数据类型：

> `int`：`python3`里没有长整型，只有`int`型
>
> `float`：浮点数
>
> `complex`：复数
>
> `bool`：布尔值，真为`True`，假为`False`
>
> `str`：字符串，文本类型，`Unicode`编码
>
> `bytes`：字节，二进制类型，



#### 注意点：

> 三元运算：
>
> ```python
> res = val1 if condition else val2
> #相当于：
> if codition:
>     res = val1
> else:
>     res = val2
> ```
>
> `str`和`bytes`：
>
> ```python
> ##bytes --> str
> b'\xe2'.decode(encoding='utf-8') #必须是能用编码方式节码的二进制
>
> ##str --> bytes
> #若全是英文，如str用utf-8编码后仅仅为b'str'
> #若不是英文，则转成二进制
> ##########################Python默认编码方式为Unicode，要转成字节必须转成utf-8
> #socket编程传输数据必须是二进制
> 'str'.encode(encoding='utf-8')
> ```
>



#### 列表`list`：

> ```python
> name = ["A", "B", "C", "D"]
>
> ###访问
> name[i-1] #第i个(从1开始计数)元素
> name[-1] #倒数第一个
> name[-2] #倒数第二个以此类推
>
> ###切片
> name[0:n] #method 1 取前n个
> name[:n] #method 2 取前n个
> name[i-1:j] #顾头不顾尾，取第i个到第j个(从1开始计数)
> name[-m:] #取最后面m个
> name[i:j:m] #以m为步长切片
>
> ###插入
> #无批量插入方法
> name.append("E") #追加
> name.insert(i-1, "F") #插入到第i个(从1开始计数)位置
>
> ###修改
> name[i-1] = "G" #修改第i个(从1开始计数)
>
> ###删除
> name.remove("A") #以内容删除，删除从左向右找到的第一个内容为'A'的数据
> del name[i-1] #以下标删除第i个元素(从1开始计数)
> name.pop(i) #以下标删除并返回第i个元素(从1开始计数)，默认参数为最后一个
>
> ###查找
> name.index("B") #以内容查找并返回索引
>
> ###统计
> name.count("C") #以内容统计重复个数
>
> ###清空列表
> name.clear()
>
> ###删除列表
> del name
>
> ###反转列表
> name.reverse()
>
> ###排序
> name.sort() #以ACSII排序
>
> ###扩展
> name.extend(name2) #name2也是一个列表，追加到name的末尾
>
> ###浅复制(name2是对name的引用，只复制一层)
> name2 = name[:] #method 1
> name2 = list(name) #method 2
> import copy
> name2 = copy.copy(name) #method 3
>
> ###深复制
> import copy
> name2 = copy.deepcopy(name) #使用copy库
>
> ###列表长度
> len(name)
> ```
>
> - `enumerate`：
>
>   > 以元组形式返回数据中的索引和值
>   >
>   > ```python
>   > A = ['a', 'b', 'c']
>   > for i in enumerate(A): 
>   >     print(i)
>   >
>   > #运行结果：
>   > (0, 'a')
>   > (1, 'b')
>   > (2, 'c')
>   > ```
>



#### 元组`tuple`：

> 特点：**一旦创建不可更改**（只读列表）
>
> 方法：
>
> > `count()`
> >
> > `index()`



#### 字符串`str`：

> ```python
> string = 'This is a Test string!'
>
> ###capitalize()
> #字符串第一个单词首字母大写，其他全小写
> string.capitalize() #>>>'This is a test string!'
>
> ###count(sub, int_s, int_e)
> #从位置int_s到位置int_e(顾头不顾尾)查找串sub，默认从头找到尾
> string.count('is', 0, -1) #>>>2
>
> ###center(int_n, one_ch)
> #一共有int_n个字符，把字符串放在中间，不够的用一个字符one_ch来填满
> string.center(31,'*') #>>>'****This is a Test string!****'
>
> ###endswith(sub, int_s, int_e)
> #从位置int_s到位置int_e(顾头不顾尾)判断该字符串是否以串sub结尾，返回bool值
> string.endswith('is', 3, 7) #>>>True
>
> ###startswith(sub, int_s, int_e)
> #从位置int_s到位置int_e(顾头不顾尾)判断该字符串是否以串sub开始，返回bool值
> string.startswith('T',11,15) #>>>False
>
> ###expandtabs(tabsize)
> #把字符串中的\t转换成tabsize个空格
> 'This\tis\ta\ttest\tstring\t'.expandtabs(1) #>>>'This is a test string '
>
> ###find(sub)
> #返回从左数第一个串sub在字符串中位置(从0开始计数)
> string.find('is') #>>>2
>
> ###rfind(sub)
> #返回从右数第一个串sub在字符串中位置(从0开始计数)
> string.rfind('is') #>>>5
>
> ###format()
> ###format_map()
> #格式化字符串
> 'my name is {name}'.format(name='Jason') #>>>'my name is Jason'
> myname={'name1':'jason','name2':'syuu'}
> print('my name is {name2}'.format_map(myname)) #>>>'my name is jason'
>
> ###isalnum()
> #判断字符串是否为阿拉伯字母（英文字母和数字），返回bool值
>
> ###isalpha()
> #判断字符串是否为纯英文字符串，返回bool值
>
> ###isdecimal()
> #判断字符串是否是十进制整数，返回bool值
>
> ###isdigit()
> #判断字符串是否是整数，返回bool值
>
> ###isidentifier()
> #判断字符串是否是一个合法的标识符，返回bool值
>
> ###islower()
> #判断字符串是否是小写，返回bool值
>
> ###isnumeric()
> #判断字符串是否是只有数字，返回bool值(有小数点都不可以)
>
> ###isspace()
> #判断字符串是否是空格，返回bool值
>
> ###istitle()
> #判断字符串中每个单词首字母是否大写，返回bool值
>
> ###title()
> #把字符串中每个单词的首字母变成大写，返回str
>
> ###isprintable()
> #判断字符串是否可以打印，返回bool值
>
> ###isupper()
> #判断字符串是否是大写，返回bool值
>
> ###join(iterable)
> #将iterable中的迭代值以字符串隔开，返回值为str值
> '-+'.join(['1', '2', '3']) #>>>'1-+2-+3'
>
> ###ljust(int_n, one_ch)
> #一共int_n个字符，字符串左对齐，右边不足补one_ch
>
> ###rjust(int_n, one_ch)
> #一共int_n个字符，字符串右对齐，左边不足补one_ch
>
> ###lower()
> #把字符串中的大写变成小写
>
> ###upper()
> #把小写变成大写
>
> ###swapcase()
> #大写变小写，小写变大写
>
> ###lstrip()
> #去掉字符串左边的回车和空格
>
> ###rstrip()
> #去掉字符串右边的回车和空格
>
> ###strip()
> #去掉字符串两边的回车和空格
>
> ###replace(old,new,count)
> #以new替换old，替换个数为前count个
>
> ###split(sub, max_n)
> #以串sub分割字符串，分割次数最大为max_n，当然串sub在分割后不会出现了，返回list
>
> ###splitlines()
> #自动以换行分割，在linux和windows换行(\n \r\n)不一样，自动识别
>
> ###zfill(int_n)
> #将字符串右对齐，左边不够以0（zero）填充
> ```
>
> - `maketrans()`：
>
>   ```python
>   p = str.maketrans('abcdefg ','12345671')
>   'This is a string!'.translate(p) #按照p的格式转换字符串
>   #>>>Thislisl1lstrin7!
>   ```
>



#### 字典`dict`：

> 特点：
>
> > - `key-value`
> >
> >
> > - 无序，无下标，以`key`查找
>
> 方法：
>
> ```python
> d = {1:'a', 2:'b', 3:'c'}
>
> ##查找
> d[2] #key值若存在则正常，若不存在则报KeyError
> d.get(2) #key只存在则输出value值，若没有输出NULL
>
> ##改
> d[2] = 'd'
>
> ##添加
> d[4] = 'e'
>
> ##删除
> del d[3] #delete 1
> d.pop(2) #delete 2 #必须有key值不然删除不了
> d.popitem() #delete 3 #随机删除一个key-value，不推荐
>
> ##判断是否存在
> print(2 in d) #返回bool值
>
> ##打印所有的values
> print(d.values())
>
> ##打印所有的keys
> print(d.keys())
>
> ##setdefault()
> #先在字典中查找key，若存在则返回value，否则将key-value插入到字典中。返回值为value
> dict.setdefault(key, value)
>
> ##update()
> #用d2去更新d1。
> #若d2中的key在d1中存在，则用d2中的value更新d1
> #若d2中的key-value在d1中没有，则增加该键值对
> d1.update(d2)
>
> ##items()
> d.items() #返回dict_item，内部是一个列表，列表的元素是元组，元组是key-value
>
> ##formkeys()
> dict.fromkeys([1,2,3],item) #用item初始化一个列表，浅复制
> ```
>
> 循环：
>
> ```python
> #推荐，快
> for i in dict_name: 
>     print(i, dict_name[i])
> #需将字典转成列表，再循环
> for k,v in dict_name.items(): 
>     print(k,v) 
> ```
>
> 字典嵌套

