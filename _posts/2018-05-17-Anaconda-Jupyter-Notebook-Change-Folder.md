---
layout： post
keys: 2018051701
tags: Win10 Anaconda Jupyter-Notebook
---

# <center>修改Win10中Anaconda Jupyter Notebook中的默认启动位置</center>

##1 临时更改启动位置

> - 打开`Anaconda Prompt`
> - 输入以下命令：`jupyter notebook [optinonal folder location]`

##2 永久更改启动位置

> - 右键`Jupyter Notebook`快捷方式，选择`Properties`
> - 修改`Target`，在`%USERPROFILE%`前面键入你需要的启动位置，如`F:\\Jupyter_notebook`，前后必须留有空格