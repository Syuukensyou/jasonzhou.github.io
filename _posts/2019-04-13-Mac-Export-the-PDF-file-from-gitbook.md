---
layout: post
keys: 20190413
tags: Mac gitbook PDF calibre
title: Mac Export the PDF file from gitbook
---

# Mac安装gitbook并导出PDF书籍

## 1、安装Node.js

建议选择LTS版本

![](https://github.com/Syuukensyou/syuukensyou.github.io/blob/master/_posts_pic/1.png?raw=true)

## 2、安装gitbook

```
sudo npm install gitbook -g
sudo npm install gitbook-cli -g
```

安装过程中可能出现各种各样的错误。列举一下：

（1）安装没有使用`sudo`命令

![](https://github.com/Syuukensyou/syuukensyou.github.io/blob/master/_posts_pic/2.png?raw=true)

（2）在上一次`npm`安装失败后没有强制清楚`cache`

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  npm cache clean --force
```

![](https://github.com/Syuukensyou/syuukensyou.github.io/blob/master/_posts_pic/3.png?raw=true)

（3）需要变换`.npm`的默认路径

更改了默认的安装路径，命令如下

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local /usr/local
╰─➤  mkdir ~/.npm-global
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  npm config set prefix '~/.npm-global'
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  export PATH=~/.npm-global/bin:$PATH
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  source ~/.profile
```

（4）中途可能会出现各种各样的错误，不过也基本上有以下几次的操作。

安装失败 —> 清楚cache —> 再次安装 —> 显示不出版本 —> 卸载 —> 安装成功，皆大欢喜

卸载：

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  sudo npm uninstall gitbook-cli -g gitbook-cli
```

安装成功：

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  gitbook -V
CLI version: 2.3.2
Installing GitBook 3.2.3
```



## 3、安装`calibre`软件

（1）拖入到Application即可

（2）敲命令：

```shell
sudo ln -s /Applications/calibre.app/Contents/MacOS/ebook-convert /usr/local/bin
```

## 4、转成PDF格式

进入所在书籍 —> `gitbook pdf` —> `npm install svgexport -g` —> `gitbook pdf`

![](https://github.com/Syuukensyou/syuukensyou.github.io/blob/master/_posts_pic/4.png?raw=true)

大功告成！！！皆大欢喜！！！

再也不用遇到好的gitbook不能本地的问题了～



参考：

[gitbook导出pdf方法详解]: https://www.jianshu.com/p/d8286217b401

