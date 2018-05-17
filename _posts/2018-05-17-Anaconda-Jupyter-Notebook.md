---
layout： post
title: Anaconda Jupyter Notebook
keys: 20180517
tags: Windows Anaconda Jupyter-Notebook
---
# <center>Win 10 Anaconda Jupyter Notebook一些操作</center>

## 1 修改启动位置

### 1.1 临时更改启动位置
> - 打开`Anaconda Prompt`
> - 输入以下命令：`jupyter notebook [optinonal folder location]`

###  1.2 永久更改启动位置

> - 右键`Jupyter Notebook`快捷方式，选择`Properties`
> - 修改`Target`，在`%USERPROFILE%`前面键入你需要的启动位置，如`F:\\Jupyter_notebook`，前后必须留有空格

## 2 转换文件格式

在`Anaconda Prompt`中转换：`ipython nbconvert --to file-type file-name.ipynb `

> 其中`file-type`可以是`markdown`、`html`、`pdf`等等，详细参考：https://ipython.org/ipython-doc/3/notebook/nbconvert.html
>

