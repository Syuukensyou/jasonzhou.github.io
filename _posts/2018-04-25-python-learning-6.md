---
layout: post
comment: true
key: 2018042502
tags: Python software-contents-structure
---



软件目录结构：

> 优点：可读性高、可维护性高
>
> ```
> Foo/
> |-- bin/
> |   |-- foo
> |
> |-- foo/
> |   |-- tests/
> |   |   |-- __init__.py
> |   |   |-- test_main.py
> |   |
> |   |-- __init__.py
> |   |-- main.py
> |
> |-- docs/
> |   |-- conf.py
> |   |-- abc.rst
> |
> |-- setup.py
> |-- requirements.txt
> |-- README
> ```
>
> 1. `bin/`: 存放项目的一些可执行文件，当然你可以起名`script/`之类的也行。
> 2. `foo/`: 存放项目的所有源代码。(1) 源代码中的所有模块、包都应该放在此目录。不要置于顶层目录。(2) 其子目录`tests/`存放单元测试代码； (3) 程序的入口最好命名为`main.py`。
> 3. `docs/`: 存放一些文档。
> 4. `setup.py`: 安装、部署、打包的脚本。
> 5. `requirements.txt`: 存放软件依赖的外部Python包列表。
> 6. `README`: 项目说明文件。
>
> 
>
> #### README
>
> **这个我觉得是每个项目都应该有的一个文件**，目的是能简要描述该项目的信息，让读者快速了解这个项目。
>
> 它需要说明以下几个事项:
>
> 1. 软件定位，软件的基本功能。
> 2. 运行代码的方法: 安装环境、启动命令等。
> 3. 简要的使用说明。
> 4. 代码目录结构说明，更详细点可以说明软件的基本原理。
> 5. 常见问题说明。
>
> 我觉得有以上几点是比较好的一个`README`。在软件开发初期，由于开发过程中以上内容可能不明确或者发生变化，并不是一定要在一开始就将所有信息都补全。但是在项目完结的时候，是需要撰写这样的一个文档的。可以参考Redis源码中[Readme](https://github.com/antirez/redis#what-is-redis)的写法，这里面简洁但是清晰的描述了Redis功能和源码结构。
>
> #### setup.py
>
> 一般来说，用`setup.py`来管理代码的打包、安装、部署问题。业界标准的写法是用Python流行的打包工具[setuptools](https://pythonhosted.org/setuptools/setuptools.html#developer-s-guide)来管理这些事情。这种方式普遍应用于开源项目中。不过这里的核心思想不是用标准化的工具来解决这些问题，而是说，**一个项目一定要有一个安装部署工具**，能快速便捷的在一台新机器上将环境装好、代码部署好和将程序运行起来。
>
> 我刚开始接触Python写项目的时候，安装环境、部署代码、运行程序这个过程全是手动完成，遇到过以下问题:
>
> 1. 安装环境时经常忘了最近又添加了一个新的Python包，结果一到线上运行，程序就出错了。
> 2. Python包的版本依赖问题，有时候我们程序中使用的是一个版本的Python包，但是官方的已经是最新的包了，通过手动安装就可能装错了。
> 3. 如果依赖的包很多的话，一个一个安装这些依赖是很费时的事情。
> 4. 新同学开始写项目的时候，将程序跑起来非常麻烦，因为可能经常忘了要怎么安装各种依赖。
>
> `setup.py`可以将这些事情自动化起来，提高效率、减少出错的概率。"复杂的东西自动化，能自动化的东西一定要自动化。"是一个非常好的习惯。
>
> setuptools的[文档](https://pythonhosted.org/setuptools/setuptools.html#developer-s-guide)比较庞大，刚接触的话，可能不太好找到切入点。学习技术的方式就是看他人是怎么用的，可以参考一下Python的一个Web框架，flask是如何写的: [setup.py](https://github.com/mitsuhiko/flask/blob/master/setup.py)
>
> #### requirements.txt
>
> 这个文件存在的目的是:
>
> 1. 方便开发者维护软件的包依赖。将开发过程中新增的包添加进这个列表中，避免在`setup.py`安装依赖时漏掉软件包。
> 2. 方便读者明确项目使用了哪些Python包。
>
> 这个文件的格式是每一行包含一个包依赖的说明，通常是`flask>=0.10`这种格式，要求是这个格式能被`pip`识别，这样就可以简单的通过 `pip install -r requirements.txt`来把所有Python包依赖都装好了。具体格式说明： [点这里](https://pip.readthedocs.org/en/1.1/requirements.html)。
>
> #### 关于配置文件的使用方法
>
> #### 注意，在上面的目录结构中，没有将`conf.py`放在源码目录下，而是放在`docs/`目录下。
>
> 很多项目对配置文件的使用做法是:
>
> 1. 配置文件写在一个或多个python文件中，比如此处的conf.py。
> 2. 项目中哪个模块用到这个配置文件就直接通过`import conf`这种形式来在代码中使用配置。
>
> 这种做法我不太赞同:
>
> 1. 这让单元测试变得困难（因为模块内部依赖了外部配置）
> 2. 另一方面配置文件作为用户控制程序的接口，应当可以由用户自由指定该文件的路径。
> 3. 程序组件可复用性太差，因为这种贯穿所有模块的代码硬编码方式，使得大部分模块都依赖`conf.py`这个文件。
>
> 所以，我认为配置的使用，更好的方式是，
>
> 1. 模块的配置都是可以灵活配置的，不受外部配置文件的影响。
> 2. 程序的配置也是可以灵活控制的。
>
> 能够佐证这个思想的是，用过`nginx`和`mysql`的同学都知道，`nginx`、`mysql`这些程序都可以自由的指定用户配置。所以，不应当在代码中直接`import conf`来使用配置文件。上面目录结构中的`conf.py`，是给出的一个配置样例，不是在写死在程序中直接引用的配置文件。可以通过给`main.py`启动参数指定配置路径的方式来让程序读取配置内容。当然，这里的`conf.py`你可以换个类似的名字，比如`settings.py`。或者你也可以使用其他格式的内容来编写配置文件，比如`settings.yaml`之类的。



获取绝对路径：

> ```python
> import os
> import sys
> os.path.abspath(__file__)  # 文件的绝对路径
> BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))  # 去除本路径获得上一级文件夹名
> sys.path.append(BASE_DIR)
> from conf import settings
> ...
> ```

