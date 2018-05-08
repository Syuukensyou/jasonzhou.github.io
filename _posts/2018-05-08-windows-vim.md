---
layout: post
key: 20180508
tags: windows vim setting
---

# <center>Windows下Vim的安装与配置</center>

## 1. 安装gVim

## 2. 配置

1. 打开`..\gVimPortable\Data\settings\_vimrc`
2. 相关设置如下：

```
" 配色方案
colorscheme desert

" 语法高亮显示
syntax on

" 显示行号
set number

" 检测文件类型
filetype on

" vim使用自动对齐，也就是把当前行的对齐格式应用到下一行
set autoindent

" 依据上面的对齐格式，只能的选择对齐方式
set smartindent

" 设置匹配模式，类似当输入一个左括号时会匹配相应的那个右括号
set showmatch

" 当vim进行编辑时，如果命令错误，会发出一个响声，该设置去掉响声
set vb t_vb=

" 在编辑过程中，在右下角显示光标位置的状态行
set ruler

" 设置游标
set cursorline

" 找要匹配的单词。eg：如果要查找search单词，当输入到/s（回车确认选择）时，会自动找到第一个s开头的单词 
set incsearch

" 设置字体和大小
set guifont=Courier_new:h14:b:cDEFAULT

" set nu! " 设置行数

" syntax enable "语法高亮


" 缩进
"设置tab宽度
set tabstop=4
"设置删除tab的宽度
set softtabstop=4 
```

