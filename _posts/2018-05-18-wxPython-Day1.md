---
layout: post
keys: 20180518
tags: wxPython Frame App Dialog
title: wxPython Day1
---

# <center>wxPython Day1</center>

## 1 实时显示窗口中鼠标的相对位置


```python
#!/bin/env python
# mouse_position.py
import wx

class MyFrame(wx.Frame):
    
    def __init__(self):
        wx.Frame.__init__(self, None, -1, "My Frame", size=(300,300))
        panel = wx.Panel(self, -1)
        panel.Bind(wx.EVT_MOTION, self.OnMove)
        wx.StaticText(panel, -1, "Pos: ", pos=(40,10))
        self.posCtrl = wx.TextCtrl(panel, -1, "", pos=(40,10))
    
    def OnMove(self, event):
        pos = event.GetPosition()
        self.posCtrl.SetValue("{x},{y}".format(x=pos.x,y=pos.y))
        

if __name__ == "__main__":
    app = wx.PySimpleApp()
    frame = MyFrame()
    frame.Show(True)
    app.MainLoop()
```

注释：第一次运行没有问题，但是第二次以及后面运行会报错，建议使用script方式运行：`python mouse_position.py`

## 2 显示图片

### 2.1 创建空的wxPython程序


```python
#!/bin/env python
# bare.py

import wx
class MyApp(wx.App):
    
    def OnInit(self):
        # parent参数必须，其余默认
        frame = wx.Frame(parent=None, title='Bare')
        frame.Show()
        return True
    
    
app = MyApp()
app.MainLoop()
```

### **每个wxPython程序必须有一个application(应用程序)对象和至少一个frame(框架)对象。application对象必须是wx.App的一个实例 或 在OnInit()方法中定义的一个子类的一个实例，OnInit()方法被wx.App父类调用**
#### 开发一个wxPython程序五个基本步骤：
- 导入wxPython包
- 子类化wxPython应用程序类
- 定义一个应用程序的初始化方法
- 创建一个应用程序类的实例
- 进入应用程序的主事件循环

### 2.2 扩展最小的空的wxPython程序


```python
#!/bin/env python
# spare.py

import wx


class MyFrame(wx.Frame):
    pass


class MyApp(wx.App):
    
    def OnInit(self):
        # parent参数必须，其余默认
        self.frame = MyFrame(parent=None, title='Bare')
        self.frame.Show()
        self.SetTopWindow(self.frame)
        return True
    
    
if __name__=='__main__':
    app = MyApp()
    app.MainLoop()
```

### 2.3 最终的hello.py


```python
#！bin/usr/bin/env python
# hello.py

import wx


class MyFrame(wx.Frame):
    """display a image"""
    
    def __init__(self, image, parent=None, id=-1, pos=wx.DefaultPosition, title='Hello, wxPython!'):
        temp = image.ConvertToBitmap()
        size = temp.GetWidth(),temp.GetHeight()  # 这是一个元祖
        wx.Frame.__init__(self, parent, id, title, pos, size)
        self.bmp = wx.StaticBitmap(parent=self, bitmap=temp)  # 该控件显示图像，要求一个位图。
        
        
class MyApp(wx.App):
    
    def OnInit(self):
        image = wx.Image('wxPython.png', wx.BITMAP_TYPE_PNG)  # wxPython.png在hello.py同路径下的一个图像文件
        self.frame = MyFrame(image)
        
        self.frame.Show()
        self.SetTopWindow(self.frame)
        return True
    
    
def main():
    app = MyApp()
    app.MainLoop()
    
    
if __name__=='__main__':
    main()
```

## 3 wxPython程序稳固的基础
### - 应用程序对象
> 管理主事件循环
> 创建一个wx.App的子类
>> - 1.定义这个子类，继承wx.App
>> - 2.在子类中写一个OnInit()方法
>> - 3.在程序的主要部分创建一个子类的实例
>> - 4.调用程序实例的MainLoop()方法，将程序的控制权交由wxPython
> 应用程序对象的生命周期
>> 开始于实例创建，结束于最后一个应用程序被关闭时结束。与Python脚本的开始结束不一样。
> 输出重定向sys.stdout，sys.stderr

### - 顶级框架/窗口对象
> 管理重要的数据，控制并呈现
> 创建窗口对象：`wx.Frame(parent, id=-1, title='', pos=wx.DefaultPosition, size=wx.DefaultSize, style=wx.DEFAULT_FRAME_STYLE, name='frame')`
> 窗口ID（窗口内不可重复）
>> - 1.明确给构造器传递一个正整数
>> - 2.使用wx.NewId()
>> - 传递一个全局变量wx.ID_ANY或-1给窗口部件的构造器

### 3.1 给框架/窗口增加窗口部件


```python
#!/usr/bin/env python
# insert_frame.py

import wx


class InsertFrame(wx.Frame):
    
    def __init__(self, parent, id):
        wx.Frame.__init__(self, parent, id, 'Frame With Button', size=(300,100))
        panel = wx.Panel(self)  # 创建画板
        button = wx.Button(panel, label='Close', pos=(125,10), size=(50,50))  # 将按钮添加到画板
        
        self.Bind(wx.EVT_BUTTON, self.OnCloseMe, button)  # 绑定按钮的点击事件
        self.Bind(wx.EVT_CLOSE, self.OnCloseWindow)  # 绑定窗口的关闭事件
        
    def OnCloseMe(self, event):
        self.Close(True)
        
    def OnCloseWindow(self, event):
        self.Destroy()
        

class MyApp(wx.App):
    
    def OnInit(self):
        self.frame = InsertFrame(parent=None, id=-1)
        self.frame.Show()
        self.SetTopWindow(self.frame)  # 设self.frame为顶级窗口
        return True
    
    
if __name__=='__main__':
    app = MyApp()
    app.MainLoop()
```

注释：第一次运行成功，第二次就会失败

### 3.2 给框架增加菜单栏、工具栏和状态栏


```python
#!usr/bin/env python
# toolbar.py

import wx

class ToolbarFrame(wx.Frame):
    
    def __init__(self, image, parent, id):
        wx.Frame.__init__(self, parent, id, 'Toolbars', size=(300,200))
        panel = wx.Panel(self)
        panel.SetBackgroundColour('White')
        
        temp = image.ConvertToBitmap()
        
        statusBar = self.CreateStatusBar()  # 创建状态栏
        toolbar = self.CreateToolBar()  # 创建工具栏
        toolbar.AddSimpleTool(wx.NewId(), temp, 'New', 'Long help for "New"')  # 给工具栏增加一个工具
        toolbar.Realize()  # 准备显示工具栏
        
        # 结构如下：
        # File
        #   - 
        # Edit
        #   - Copy
        #   - Cut
        #   - Paste
        #   - ---
        #   - Options...
        # *****************
        menuBar = wx.MenuBar()  # 创建菜单栏
        
        # 创建文件栏和编辑栏
        menuFile = wx.Menu()
        menuEdit = wx.Menu()
        
        # 在编辑栏附上项目
        menuEdit.Append(wx.NewId(), '&Copy', 'Copy in status bar')
        menuEdit.Append(wx.NewId(), 'C&ut', '')
        menuEdit.Append(wx.NewId(), 'Paste', '')
        menuEdit.AppendSeparator()
        menuEdit.Append(wx.NewId(), '&Options...', 'Display Options')
        
        menuBar.Append(menuFile, '&File')  # 在菜单栏上附上文件栏
        menuBar.Append(menuEdit, '&Edit')  # 在菜单栏上附上编辑栏
        
        self.SetMenuBar(menuBar)  # 在框架上附上菜单栏
        
        
class MyApp(wx.App):
    
    def OnInit(self):
        image = wx.Image('wxPython.png', wx.BITMAP_TYPE_PNG)
        self.frame = ToolbarFrame(image, parent=None, id=-1)
        self.frame.Show()
        return True

    
if __name__=='__main__':
    app = MyApp()
    app.MainLoop()
```

### 3.3 对话框
#### 有许多种类对话框，如下：
> - 消息对话框
> - 文本输入对话框
> - 列表选择
> - 文件选择器
> - 色彩选择器
> - 进度对话框
> - 打印设置
> - 字体选择器

#### 3.3.1 消息对话框


```python
wx.MessageDialog(parent, message, caption='Message box', style=wx.OK|wx.CANCEL, pos=wx.DefaultPosition)
```


```python
dlg = wx.MessageDialog(None, 'Is this the coolest thing ever!', 'MessageDialog', wx.YES_NO|wx.ICON_QUESTION)
result = dlg.ShowModel()  # 将对话框以模式框架的方式显示
dlg.Destroy()
```

#### 3.3.2 文本输入对话框


```python
wx.TextEntryDialog(parent, message, caption='Please enter text', value='', style=wx.OK|wx.CANCEL, pos=wx.DefaultPosition)
```


```python
dlg = wx.TextEntryDialog(None, "Who is buried in Grant's tomb", 'A Question', 'Cary Grant')
if dlg.ShowModel == wx.ID_OK:  # ShowModel返回所摁下按钮的ID
    response = dlg.GetValue()  # 获取用户输入，SetValue设置输入
```

#### 3.3.3 列表选择


```python
wx.SingleChoiceDialog(parent, message, caption, choices, style=wx.CHOICEDLG_STYLE, pos=wx.DefaultPosition)
```


```python
dlg = wx.SingleChoiceDialog(None, 'What version of Python are you using?', 'Single Choice', ['2.7', '3.5'])
if dlg.ShowModel() == wx.ID_OK:
    response = dlg.GetStringSelection()  # GetSelection()返回用户选项的索引，GetStringSelection返回用户选择的字符串
```
