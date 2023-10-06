---
title: 教程|Python安装指南
date: '2023/10/5 17:00'
categories: 教程
keywords: Python
description: 安装Python、配置Python环境（附安装包）
abbrlink: c34e2ac0
toc:
  - Python
  - 教程
updated:
cover:
tags:
sticky:
swiper_index:
---

## 1. 选择正确的版本，下载安装包
根据你的实际需要选择Python发行版本。值得注意的是，编程语言包并不是越新越好的，不同版本的Python之间可能会产生兼容性问题。
如果你不确定你的项目需要哪个版本，请查阅您可能需要使用到的插件的文档或咨询有相关经验的前辈。

- 注：Python于10月2日发布3.12.0版本，站长在10月5日安装项目依赖包时产生错误，故本教程采用先前成功过的Python3.11.5版本。

Python 3.11.5官方下载链接：[Python 3.11.5](https://www.python.org/downloads/release/python-3115/)
拉到页面底部，**根据你的系统选择对应的版本下载**，站长使用的是64位Windows系统，国内大多数普通用户使用的一般也是这个版本。
![Download](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_01.png)

国内下载速度可能会有点慢，有条件的可以通过国际网络访问
站长这里提供Python 3.11.5 Windows64位版本的下载：[python-3.11.5-amd64](http://cclmsy.gitee.io/sourse/Tools/python-3.11.5-amd64.exe)

## 2. 安装Python到计算机
下载完成后，打开文件所在的文件夹，选中文件右键，以管理员身份运行，按下图所示的步骤安装
![Install1](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_02.png)
![Install2](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_03.png)
![Install3](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_04.png)
![Install4](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_05.png)
![Install5](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_06.png)

## 3. 检查环境变量
1. 打开设置，搜索“环境变量”，点击“编辑账户的环境变量”
![Env1](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_07.png)
2. 在弹出的页面单击选中“Path”，点击“编辑”
![Env2](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_08.png)
3. 在弹出的页面查看是否包含这两项。若没有，则添加这两项。（如果你在上一步自定义了安装路径，则添加对应的路径）
![Env3](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_09.png)
4. 依次点击上面两个弹窗的“确定”，才能正确保存PATH。如果不确定是否正确保存，可以回去看一下//

## 4. 检查是否正确安装了Python
1. 键盘按Windows+R，输入cmd运行终端
![Check1](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_10.png)
2. 输入python回车，若显示版本号则表示安装成功
![Check2](http://cclmsy.gitee.io/sourse/Images/BOT/BOT_11.png)
