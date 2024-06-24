---
title: 教程|Windows系统无法激活Python虚拟环境的解决方案
date: '2023/10/5 21:00'
categories: 教程
keywords: Python
abbrlink: Tutor_Python_venv_activate
tags:
  - Python
  - 教程
description: 教程|Windows系统无法激活Python虚拟环境的解决方案
updated:
cover:
sticky:
swiper_index:
---

# 情况描述
在Windows系统终端激活Python虚拟环境时可能出现以下报错（假设你的虚拟环境名为“.venv”）：
```
PS (yourpath\yourProj)> .\.venv\Scripts\activate
.\.venv\Scripts\activate : 无法加载文件 (yourpath\yourProj)\.venv\Scripts\activate.ps1，因为在此系统上禁止运行脚本。
有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ .\.venv\Scripts\activate
+ ~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```
# 报错原因
Windows系统具有执行策略（Execution Policy），导致无法运行虚拟环境的激活脚本。
执行策略是Windows中的一种安全机制，它可以防止执行不信任的脚本。
# 解决方案
1. 点击屏幕左下角Windows徽标打开“开始”菜单栏，在搜索栏搜索“PowerShell”，然后单击 “以管理员身份运行”。
![Python_venv1](https://source.cclmsy.cc/Images/Posts/BOT/BOT_19.png)
2. 在弹出的终端中输入以下命令，回车：
```
Set-ExecutionPolicy RemoteSigned
```
3. 弹出询问时输入“y”，回车：
![Python_venv2](https://source.cclmsy.cc/Images/Posts/BOT/BOT_20.png)
4. 没有消息提示，退出 PowerShell 终端，回到你激活Python虚拟环境的终端。此时可以正常激活Python虚拟环境：
```
PS (yourpath\yourProj)> .\.venv\Scripts\activate
(yourProj) PS (yourpath\yourProj)> 
```
