---
layout: post
title:Python Learning 1
key:2018040401
tags: Python getpass
---

若要生成可执行程序，必须声明解释器

> `#!usr/bin/env python`在系统中找python的环境变量
>
> `#!usr/bin/python`使用系统原装的python环境变量，不推荐使用



变量：

> `name1 = "A" #name1指向A`
>
> `name2 = name1 #name2指向name1指向的地址，即指向A`
>
> `name1 = "C" #name1指向C，而name2还是指向A`
>
> 小写加下划线命名

常量：

> 大写命名，默认不要改动！
>
> Python中没有常量的定义



注释：

> 当行注释`#`
>
> 多行注释`''' ''''`

打印：

> 多行打印`''' '''`
>
> 单行打印`""`或`''`



输入：

> `name = input("input a name")`
>
> 强制转换：`age = int(input("age:"))`

格式化输出：

> `print("info of %s: name: %s, age: %d" % (name, name, age))`
>
> `print("info of {_name}: name: {_name}, age:{_age}".format(_name=name, _age=age))`
>
> `print("info of {0}: name: {0}, age:{1}".format(name, age)) #不推荐`



密文(pycharm不好使，dos可以)：

> `import getpass`
>
> `passwd = getpass.getpass("password:")`



条件：

> `if...else...`
>
> `while...else... #当条件为真则执行while下面的语句，当条件为假则执行else下面的语句`

