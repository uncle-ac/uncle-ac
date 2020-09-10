---
title: python_arrange
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/77.webp'
date: 2020-09-03 18:43:13
authorLink:
categories:
tags: python
keywords:
description: 用OS模块写了一个整理文件夹内容的程序
---

>写这个代码花了一个小时，因为刚学了一点点OS模块，还不是很熟悉，写完后功能也都有，但是！！！导出为exe文件时不小心误删了py文件，气死我了！又重新写了一遍。
>
>新加了GUI界面的整理页面小程序，效果如下图所示
>
>GitHub项目地址 [Here](https://github.com/uncleacc/Organize-documents)

**效果图：**

{% folding, GUI界面 %} {% fb_img http://ww1.sinaimg.cn/large/007Y60soly1gilvhm4w0ij30oa0gn0to.jpg %} {% endfolding %}

{% folding, 黑窗口 %} {% fb_img http://ww1.sinaimg.cn/large/007Y60soly1gijcql8z7pj30ln0bhgmc.jpg %} {% endfolding %}

{% folding, 实现效果 %} {% fb_img http://ww1.sinaimg.cn/large/007Y60soly1gijdfc5nb5j30sv084ab9.jpg %} {% endfolding %}



## CODE1（不含有重命名）

```python
import os
from shutil import copyfile

def create():
    print('------创建文件夹------')
    for i in var_list:
        Path_now = os.path.join(Path, i).replace('\\', '/')
        if(not os.path.exists(Path_now)):
            os.mkdir(Path_now)
    print('------创建完成------')

def move():
    print('------移动文件------')
    for file in os.listdir(Path):
        Path_src = os.path.join(Path, file).replace('\\', '/')
        if(os.path.isfile(Path_src)):
            pos = file.find('.') + 1
            postfix = file[pos:]
            if(postfix in var_list):
                Path_tar = os.path.join(Path, postfix, file).replace('\\', '/')
                copyfile(Path_src, Path_tar)
                os.remove(Path_src)
    print('------移动完成------')

var_list = ['jpg', 'jpeg', 'png', 'txt', 'webp', 'gif', 'avi'] #改这里面的内容可以添加整理文件方式(只需要改这里)
Path = input('请输入您要整理的文件夹目录位置： ')
name = input('请输入您要整理的文件夹名称： ')
Path = os.path.join(Path, name).replace('\\', '/')
print(Path)
sure = input('确认您要整理此文件夹吗？ y/n?')
if(sure == 'y'):
    flag = 0
    try:
        flag = os.path.isdir(Path)
    except:
        print('无法打开此文件夹！请确认你具有权限访问此文件夹！！！')
    if(flag):
        print('------开始整理------')
        create()
        move()
    else:
        print('请确认地址指向文件夹')
else:
    print('------exit successfully------')

os.system("pause")
```

## CODE2 （重命名）

实现了移动文件的过程中从0开始往上迭代重命名的过程

```python
# coding=utf-8
import os
from shutil import copyfile
from typing import Any

# 全局变量
cnt_jpg = cnt_gif = cnt_png = cnt_txt = cnt_avi = cnt_webp = cnt_pdf = cnt_doc = 0
fileFormat = ['jpg', 'jpeg', 'png', 'txt', 'webp', 'gif', 'avi'] # 分类的文件格式
Path = ""
name = ""
prefix = ""

# 用户输入
def __input():
    global Path, name, prefix
    Path = input('请输入您要整理的文件夹目录位置： ')
    name = input('请输入您要整理的文件夹名称： ')
    prefix = input('请输入重命名后的文件前缀： ')
    Path = os.path.join(Path, name).replace('\\', '/')

# 创建文件夹，如果文件夹中有文件则将这些文件夹移出来
def __create():
    print('------创建文件夹------')
    for i in fileFormat:
        Path_now = os.path.join(Path, i).replace('\\', '/')
        if(not os.path.exists(Path_now)):
            os.mkdir(Path_now)
        else:
            for file in os.listdir(Path_now):
                Path_cur = os.path.join(Path_now, file).replace('\\', '/')
                Path_tar = os.path.join(Path, file).replace('\\', '/')
                copyfile(Path_cur, Path_tar)
                os.remove(Path_cur)
    print('------创建完成------')

# 重命名
def __change(name, var, pos):
    global cnt_jpg, cnt_gif, cnt_png, cnt_txt, cnt_avi, cnt_webp, cnt_doc, cnt_pdf
    if(var == 'jpg'):
        cnt_jpg += 1
        name = name.replace(name[0:pos-1], str(cnt_jpg))
    if(var == 'avi'):
        cnt_avi += 1
        name = name.replace(name[0:pos - 1], str(cnt_avi))
    if(var == 'doc'):
        cnt_doc += 1
        name = name.replace(name[0:pos - 1], str(cnt_doc))
    if(var == 'pdf'):
        cnt_pdf += 1
        name = name.replace(name[0:pos - 1], str(cnt_pdf))
    if(var == 'webp'):
        cnt_webp += 1
        name = name.replace(name[0:pos - 1], str(cnt_webp))
    if(var == 'png'):
        cnt_png += 1
        name = name.replace(name[0:pos-1], str(cnt_png))
    if(var == 'gif'):
        cnt_gif += 1
        name = name.replace(name[0:pos - 1], str(cnt_gif))
    if(var == 'txt'):
        cnt_txt += 1
        name = name.replace(name[0:pos - 1], str(cnt_txt))
    return prefix + name

def __move():
    print('------移动文件------')
    for file in os.listdir(Path):
        Path_src = os.path.join(Path, file).replace('\\', '/')
        if(os.path.isfile(Path_src)):
            pos = file.rfind('.') + 1
            postfix = file[pos:]
            if(postfix in fileFormat):
                file = __change(file, postfix, pos)
                Path_tar = os.path.join(Path, postfix, file).replace('\\', '/')
                copyfile(Path_src, Path_tar)
                os.remove(Path_src)
    print('------移动完成------')

def main():
    __input()
    print(Path)
    sure = input('确认您要整理此文件夹吗？ y/n?')
    if(sure == 'y'):
        if(os.path.isdir(Path)):
            print('------开始整理------')
            __create()
            __move()
        else:
            print('请确认地址指向文件夹，如果确实是文件夹则检查权限问题')
    else:
        print('------exit successfully------')


main()
os.system("pause")
```

## CODE3 （GUI界面）

```python
# coding=utf-8
import os
import tkinter as tk
import tkinter.messagebox
from shutil import copyfile
from typing import Any

#创建tk窗口
window = tk.Tk()
window.title('文档整理器')
window.geometry('700x450')
#建立表单
var_info = tk.StringVar()
var_info.set('当前您还没有进行整理')
tk.Label(window, font=('Arial, 15'), bg='yellow', textvariable=var_info).place(x=250, y=20)
tk.Label(window, font=('Arial', 15), text='请输入您要整理的文件夹的路径: ').place(x=10, y=100)
tk.Label(window, font=('Arial', 15), text='请输入该文件夹的名字: ').place(x=10, y=150)
tk.Label(window, font=('Arial', 15), text='请输入重命名后的文件前缀： ').place(x=10, y=200)
# 创建输入框
# 文件夹路径
var_Path = tk.StringVar()
entry_Path = tk.Entry(window, font=('Arial',15), textvariable=var_Path, width=30)
entry_Path.place(x=300, y=100)
# 文件夹名称
var_name = tk.StringVar()
entry_Name = tk.Entry(window, font=('Arial',15), textvariable=var_name, width=30)
entry_Name.place(x=300, y=150)
# 重命名前缀
var_prefix = tk.StringVar()
entry_Prefix = tk.Entry(window, font=('Arial',15), textvariable=var_prefix, width=30)
entry_Prefix.place(x=300, y=200)

# 全局变量
cnt_jpg = cnt_gif = cnt_png = cnt_txt = cnt_avi = cnt_webp = cnt_pdf = cnt_doc = 0
fileFormat = ['jpg', 'jpeg', 'png', 'txt', 'webp', 'gif', 'avi'] # 分类的文件格式
Path = ''
name = ''
prefix = ''
# 创建文件夹，如果文件夹中有文件则将这些文件夹移出来
def __create():
    global var_info
    var_info.set('------创建文件夹------')
    for i in fileFormat:
        Path_now = os.path.join(Path, i).replace('\\', '/')
        if(not os.path.exists(Path_now)):
            os.mkdir(Path_now)
        else:
            for file in os.listdir(Path_now):
                Path_cur = os.path.join(Path_now, file).replace('\\', '/')
                Path_tar = os.path.join(Path, file).replace('\\', '/')
                copyfile(Path_cur, Path_tar)
                os.remove(Path_cur)
    var_info.set('------创建完成------')

# 重命名
def __change(name, var, pos):
    global cnt_jpg, cnt_gif, cnt_png, cnt_txt, cnt_avi, cnt_webp, cnt_doc, cnt_pdf
    if(var == 'jpg'):
        cnt_jpg += 1
        name = name.replace(name[0:pos-1], str(cnt_jpg))
    if(var == 'avi'):
        cnt_avi += 1
        name = name.replace(name[0:pos - 1], str(cnt_avi))
    if(var == 'doc'):
        cnt_doc += 1
        name = name.replace(name[0:pos - 1], str(cnt_doc))
    if(var == 'pdf'):
        cnt_pdf += 1
        name = name.replace(name[0:pos - 1], str(cnt_pdf))
    if(var == 'webp'):
        cnt_webp += 1
        name = name.replace(name[0:pos - 1], str(cnt_webp))
    if(var == 'png'):
        cnt_png += 1
        name = name.replace(name[0:pos-1], str(cnt_png))
    if(var == 'gif'):
        cnt_gif += 1
        name = name.replace(name[0:pos - 1], str(cnt_gif))
    if(var == 'txt'):
        cnt_txt += 1
        name = name.replace(name[0:pos - 1], str(cnt_txt))
    return prefix + name

def __move():
    var_info.set('------移动文件------')
    for file in os.listdir(Path):
        Path_src = os.path.join(Path, file).replace('\\', '/')
        if(os.path.isfile(Path_src)):
            pos = file.rfind('.') + 1
            postfix = file[pos:]
            if(postfix in fileFormat):
                file = __change(file, postfix, pos)
                Path_tar = os.path.join(Path, postfix, file).replace('\\', '/')
                copyfile(Path_src, Path_tar)
                os.remove(Path_src)
    var_info.set('------移动完成------')

def check():
    if(var_Path.get() and var_name.get()):
        return True

def main():
    if(check()):
        global Path, prefix
        Path = os.path.join(var_Path.get(), var_name.get()).replace('\\', '/')
        prefix = var_prefix.get()
        sure = tk.messagebox.askyesno(title='Are you sure?', message='请确定您要整理的文件夹路径是{}'.format(Path))
        if(sure == True):
            if(os.path.isdir(Path)):
                var_info.set('------开始整理------')
                __create()
                __move()
                tk.messagebox.showinfo(title='Success', message='文件整理完毕')
            else:
                tk.messagebox.showerror(title='ERROR', message='请确认该路径指向文件夹，如果确实是文件夹则检查权限问题')
    else:
        tk.messagebox.showwarning(title='Warning', message='请确保您的文件夹目录和名字进行了输入(前缀为空则默认全部数字)')

bt1 = tk.Button(window,font=('Arial',15), activebackground='black', text='开始整理', height=2, width=30, command=main)
bt1.place(x=210, y=260)



window.mainloop()
```

