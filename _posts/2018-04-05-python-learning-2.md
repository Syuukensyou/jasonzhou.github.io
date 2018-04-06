---
layout: post
key: 20180405
tags: Python list dict str
modify_date: 2018-04-05
---

#### 标准库：

> 1. `sys`
>
>    > `sys.path`：Python使用的库存放在路径之一中，即环境变量
>    >
>    > `sys.avgr`：打印脚本的相对路径
>    >
>    > - `sys.avgr[i]`：可以取位置（从0开始）为i的输入参数
>
> 2. `os`（...为系统相关命令）
>
>    > `os.system("...")`：执行系统命令，命令返回值为0即成功，不保存结果
>    >
>    > `os.popen("...")`：执行系统命令，命令返回值为内存地址
>    >
>    > - `os.popen("...").read()`：读取内容，保存命令执行结果
>    >
>    > `os.mkdir("...")`：创建新目录



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

> 三元运算：`res = val1 if condition else val2`
>
> > - `condition == True`->`res = val1`
> > - `condition == False`->`res = val2`
>
> `str`和`bytes`：
>
> >- `bytes` -> `str`：`b'\xe2'.decode(encoding='utf-8')`
> >
> >  > 必须是能用编码方式节码的二进制
> >
> >- `str` -> `bytes`：`'str'.encode(encoding='utf-8')` 
> >
> >  > - socket编程传输数据必须是二进制
> >  > - 若全是英文，如`str`用`utf-8`编码后仅仅为`b'str'`
> >  > - 若不是英文，则转成二进制
> >  > - 默认编码方式为`utf-8`
> >
> >![str-bytes](F:\Github\LearnPython\str-bytes.png)



#### 列表`list`：

> - `name = ["A", "B", "C", "D"]`
>
>
> - 访问：
>
>   > - 第i个(从1开始计数)元素：`name[i-1]`
>   > - 倒数第一个：`name[-1]`
>   > - 倒数第二个：`name[-2]`#以此类推
>
>
> - **切片**：
>
>   > - 取前n个：`name[0:n]`或是`name[:n]`
>   > - 取第i个到第j个(从1开始计数)：`name[i-1:j]`（顾头不顾尾）
>   > - 取最后面m个：`name[-m:]`
>   > - 有步长的切片：`name[i:j:m] #以m为步长切片`
>
>
> - 插入：
>
>   > 追加：`name.append("E")`
>   >
>   > 插入第i个(从1开始计数)位置：`name.insert(i-1, "F")`
>   >
>   > **不能批量插入**
>
>
> - 修改：第i个(从1开始)`name[i-1] = "G"`
>
> - 删除：
>
>   > 以内容删除：`name.remove("A")`
>   >
>   > 以下标删除第i个元素(从1开始计数)：`del name[i-1]`
>   >
>   > 以下标返回删除并**返回**第i个元素(从1开始计数)：`name.pop(i)`
>
> - 查找(以内容查找并返回索引)：`name.index("B")`
>
> - 统计(以内容统计重复个数)：`name.count("C")`
>
> - 清空列表：`name.clear()`
>
> - 删除列表：`del name`
>
> - 反转：`name.reverse()`
>
> - 排序(以`ACSII`排序)：`name.sort()` 
>
> - 扩展：`name.extend(name2) #name2也是一个列表，追加到name的末尾`
>
>
> - 浅复制(name2是对name的引用，只复制一层)：（可以用于创造联合账号）
>
>   > - `name2 = name[:]`
>   > - `name2 = list(name)`
>   > - `name2 = copy.copy() #使用copy库`
>
> - 深复制：`copy.deepcopy() #使用copy库`
>
> - `enumerate`：
>
>   > 以元组形式返回数据中的索引和值
>   >
>   > `ex:`
>   >
>   > > `A = ['a', 'b', 'c']`
>   > >
>   > > `for i in enumerate(A): print(i)`
>   > >
>   > > 运行结果:
>   > >
>   > > > (0, 'a')
>   > > >
>   > > > (1, 'b')
>   > > >
>   > > > (2, 'c')
>
> - `len`：
>
>   > 返回列表长度：`len(name)`



#### 元组`tuple`：

> 特点：**一旦创建不可更改**（只读列表）
>
> 方法：
>
> > `count()`
> >
> > `index()`



#### 字符串`str`：

> - `capitalize()`：首字母大写
>
>
> - `count(sub, int_s, int_e)`：从位置`int_s`到位置`int_e`(顾头不顾尾)查找串`sub`，默认从头找到尾
>
> - `center(int_num, one_charater)`：一共打印`int_num`个字符，把字符串放在中间，不够的用**一个字符**`one_charater`来填满
>
> - `endswith(sub, int_s, int_e)`：从位置`int_s`到位置`int_e`(顾头不顾尾)判断该字符串是否以串`sub`结尾，返回`bool`值
>
> - `startswith(sub, int_s, int_e)`：从位置`int_s`到位置`int_e`(顾头不顾尾)判断该字符串是否以串`sub`开始，返回`bool`值
>
> - `expandtabs(tabsize)`：把字符串中的`\t`转换成`tabsize`个空格
>
> - `find(sub)`：返回从左数第一个串`sub`在字符串中位置(从0开始计数)
>
> - `rfind(sub)`：返回从右数第一个串`sub`在字符串中位置(从0开始计数)
>
> - `format()`与`format_map()`：自我探索吧~
>
> - `isalnum()`：判断字符串是否为阿拉伯数字，返回`bool`值
>
> - `isalpha()`：判断字符串是否为纯英文字符串，返回`bool`值
>
> - `isdecimal()`：判断字符串是否是十进制，返回`bool`值
>
> - `isdigit()`：判断字符串是否是整数，返回`bool`值
>
> - `isidentifier()`：判断字符串是否是一个合法的**标识符**，返回`bool`值
>
> - `islower()`：判断字符串是否是小写，返回`bool`值
>
> - `isnumeric()`：判断字符串是否是只有数字，返回`bool`值
>
> - `isdecimal()`：判断字符串是否是十进制，返回`bool`值
>
> - `isspace()`：判断字符串是否是空格，返回`bool`值
>
> - `istitle()`：判断每个单词首字母是否大写，返回`bool`值
>
> - `title()`：把每个单词的首字母变成大写，返回`str`
>
> - `isprintable()`：判断字符串是否可以打印，返回`bool`值
>
> - `isupper()`：判断字符串是否是大写，返回`bool`值
>
> - `join(iterable)`：将`iterable`中的迭代值以字符串隔开，返回值为`str`值
>
> - `ljust(int_n, one_ch)`：打印`int_n`个字符，字符串左对齐，右边不足补`one_ch`
>
> - `rjust(int_n, one_ch)`：打印`int_n`个字符，字符串右对齐，左边不足补`one_ch`
>
> - `lower()`：把字符串中的大写变成小写
>
> - `upper()`：把小写变成大写
>
> - `swapcase()`：大写变小写，小写变大写
>
> - `lstrip()`：去掉字符串左边的回车和空格
>
> - `rstrip()`：去掉字符串右边的回车和空格
>
> - `strip()`：去掉字符串两边的回车和空格
>
> - `maketrans()`：
>
>   > `ex`：
>   >
>   > > `p = str.maketrans('abcdefg ','12345671')`
>   > >
>   > > `'This is a string!'.translate(p)`
>   > >
>   > > `output`:`Thislisl1lstrin7!`
>   > >
>   > > 注：按照p的格式转换字符串
>
> - `replace(old,new,count)`：以`new`替换`old`，替换个数为前`count`个
>
> - `split(sub, max_n)`：以串`sub`分割字符串，分割次数最大为`max_n`，当然串`sub`在分割后不会出现了，返回`list`
>
> - `splitlines()`：自动以换行分割，在`linux`和`windows`换行(`\n` `\r\n`)不一样，自动识别
>
> - `zfill(int_n)`：将字符串右对齐，左边不够以`0`（zero）填充



#### 字典`dict`：

> 特点：
>
> > - `key-value`
> >
> >
> > - 无序，无下标，以`key`查找
>
> 方法`d = {1:'a', 2:'b', 3:'c'}`：
>
> > - 查：
> >
> >   > `d[2] #key值若存在则正常，若不存在则报KeyError`
> >   >
> >   > `d.get(2) #key只存在则输出value值，若没有输出NULL`
> >
> > - 改：`d[2] = 'd'`
> >
> >
> > - 添加：`d[4] = 'e'`
> >
> >
> > - 删除：
> >
> >   > `del d[3]`
> >   >
> >   > `d.pop(2) #必须有key值不然删除不了`
> >   >
> >   > `d.popitem() #随机删除一个key-value，不推荐`
> >
> >
> > - 判断是否存在：`2 in d #返回bool值`
> > - 打印所有的`value`：`d.values()`
> > - 打印所有的`key`：`d.keys()`
> > - `setdefault(key, value)`：先在字典中查找`key`，若存在则返回`value`，否则将`key-value`插入到字典中。返回值为`value`
> > - `d1.update(d2)`：用`d2`去更新`d1`。若`d2`中的`key`在`d1`中存在，则用`d2`中的`value`更新`d1`；若`d2`中的`key-value`在`d1`中没有，则增加该键值对
> > - `items()`：返回`dict_item`，内部是一个列表，列表的元素是元组，元组是`key-value`
> > - `dict.fromkeys([1,2,3],item)`：初始化一个列表，浅复制
>
> 循环：
>
> > `for i in dict_name: print(i, dict_name[i]) #推荐，快`
> >
> > `for k,v in dict_name.items(): print(k,v) #需将字典转成列表，再循环`
>
> 字典嵌套

