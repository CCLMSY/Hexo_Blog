---
title: 教程|本地化部署一个QQ机器人
date: '2023/10/6 14:00'
categories: 教程
keywords:
  - Python
  - 机器人
  - 教程
abbrlink: 11f0411
tags:
  - Python
  - 机器人
  - 教程
description: 拥有一个自己的QQ机器人难道不是一件很酷的事情吗//
updated:
cover:
sticky: 10
swiper_index: 10
---

# 0.序言
1. 本教程主要面向**Windows系统**用户
2. 教程从0开始全程详细指导，0基础萌新请放心食用🍕
3. 如果你遇到了问题，请仔细检查是否哪一步有遗漏。
4. 如果你确定自己的操作没问题，可以在评论区里提问。注意留言时请填写正确的邮箱以确保能收到站长的回复。

# 1.搭建前的准备工作：
## 1.1.需要用到的工具
- 一台24小时开机联网的服务器/计算机
- 操作系统：Windows10
- 文本编辑器（强烈推荐使用VSCode）
- Python3.11.5
- 一个闲置的QQ账号

## 1.2.Python安装指南
参考我的这篇文章：[Python安装指南](/posts/c34e2ac0.html)

10月2日发布了Python3.12.0版本，站长在10月5日安装机器人框架依赖时会报以下错误：
```
ERROR: Could not build wheels for yarl, httptools, multidict, which is required to install pyproject.toml-based projects
安装依赖失败! 请手动安装依赖.
```
尝试了几种方法没有解决，故使用之前成功过的Python3.11.5版本。

Python 3.11.5官方下载链接：[Python 3.11.5](https://www.python.org/downloads/release/python-3115/)
国内下载速度可能会有点慢，有条件的可以通过国际网络访问。
站长这里提供Windows64位版本的下载：[python-3.11.5-amd64](http://source.cclmsy.cc/Tools/python-3.11.5-amd64.exe)

## 1.3.VSCode安装指南
VSCode是一款轻量化的IDE，支持市面上绝大多数编程语言，具有良好的扩展性，且具有非常友好的代码补全功能。~~是CC站长的最爱~~
下载完成后直接一路确定傻瓜式安装即可//部分自定义配置说明如下：
![VSCode](http://source.cclmsy.cc/Images/BOT/BOT_12.png)

VSCode官网：[Visual Studio Code](https://code.visualstudio.com/)
同样国内下载比较慢，站长这里提供Windows64位1.82.2版本的下载，VSCode支持软件内更新到最新版本：[VSCodeUserSetup-x64-1.82.2](http://source.cclmsy.cc/Tools/VSCodeUserSetup-x64-1.82.2.exe)

# 2.安装Nonebot框架
## 2.1.创建文件夹
创建一个文件夹用于存放项目，然后用VSCode打开这个文件夹。
![InstallNonebot1](http://source.cclmsy.cc/Images/BOT/BOT_13.png)
## 2.2.安装pipx
点击“终端-新建终端”（或直接按住Ctrl+Shift+`）创建一个终端，在终端中输入以下命令：
```
python -m pip install --user pipx
python -m pipx ensurepath
```
如果出现以下提示，请调整你的账户环境变量：
![InstallNonebot2](http://source.cclmsy.cc/Images/BOT/BOT_14.png)
调整账户的环境变量参考：[Python安装指南](/posts/c34e2ac0.html)

**安装完成后，关闭当前VSCode窗口，重启VSCode后才会生效。**
## 2.3.安装Nonebot脚手架
创建一个终端，在终端中输入以下命令：
```
pipx install nb-cli
```
![InstallNonebot3](http://source.cclmsy.cc/Images/BOT/BOT_15.png)
如果出现无法找到`pipx`命令的情况（例如出现“Command not found”字样），请确认你在上一步中账户环境变量设置正确，且**完全重启**VSCode。
如果你确信自己的所有配置完全正确，请尝试重启你的计算机。

# 3.创建项目
在终端中输入以下命令：
```
nb create
```
选择“bootstrap (初学者或用户)”，回车。
![Create1](http://source.cclmsy.cc/Images/BOT/BOT_16.png)

等待资源加载，加载完成后进行如下配置：
 (键盘上下键移动光标，空格选中，回车确认)
```
[?] 项目名称: [为你的项目取个名字吧！] (项目名称建议由英文字母和数字构成，因为部分插件可能不支持包含中文的路径)
[?] 要使用哪些驱动器? FastAPI (FastAPI 驱动器) 
[?] 要使用哪些适配器? OneBot V11 (OneBot V11 协议) 
[?] 立即安装依赖? y
[?] 创建虚拟环境? y
```
![Create2](http://source.cclmsy.cc/Images/BOT/BOT_17.png)
脚手架会自动创建并引入所需的依赖包。

最后选择安装内置插件`echo`，这是一个简单的复读回显插件，可以用于测试你的机器人是否正常运行：
```
[?] 要使用哪些内置插件? echo
```
请注意，如果你这一步没有安装`echo`，**后续将无法进行补装**。
如果你操作失误没有安装，请删除当前项目文件夹并返回第`3`节开头。

出现以下提示则项目创建成功
![Create3](http://source.cclmsy.cc/Images/BOT/BOT_18.png)

在终端中输入以下命令进入项目文件夹：
```
cd [你的项目的名字]
```
**请注意：后续的操作均需要你的终端处于项目文件夹中。**
如果你使用的是VSCode，方便起见，你可以直接选择打开项目文件夹作为VSCode工作文件夹：
![Create4](http://source.cclmsy.cc/Images/BOT/BOT_22.png)
这样后续VSCode新建终端就会默认处于项目文件夹中。

# 4.编辑配置文件
脚手架会在项目文件夹中自动创建一个配置文件`.env.prod`，但你仍然需要进行一些自定义配置。

打开项目文件夹下的`.env.prod`文件，编辑内容如下：
```
DRIVER=~fastapi  # 脚手架自动生成的配置

HOST=127.0.0.1  # 配置 NoneBot 监听的 IP / 主机名
PORT=23333  # 配置 NoneBot 监听的端口
```
其中`HOST`**必须**设为`127.0.0.1`（即localhost）
`PORT`可以选择你喜欢的数字，范围在0-65535之间。不设置默认为8080，但尽量在15000-50000之间选择，避免和已有的端口发生冲突。

此外，你可以选择设置超级用户`SUPERUSERS`、命令起始字符`COMMAND_START`和命令分割字符`COMMAND_SEP`，你可以在有需要使用这些设置的时候再行配置。
部分插件可能会区分普通用户和超级用户（管理员），可以在`SUPERUSERS`中设置超级用户
设置命令起始字符后，机器人只响应以起始字符开头的命令。
设置命令分割字符后，你可以用一个语句包含多个命令，机器人会先按分割字符将命令分开，再按顺序处理。
```
SUPERUSERS=["1234567890","2333333333"]  # 配置 NoneBot 的超级用户，可以是多个（你的QQ号）（可选）
COMMAND_START=["/"]  # 配置命令起始字符（可选）
COMMAND_SEP=["."]  # 配置命令分割字符（可选）
```

# 5.安装插件
## 5.1.管理插件的方法
> 本节旨在让读者了解操作管理插件的流程，选择自己喜欢的管理插件的方式。
> 你也可以选择先跳过本节，在后面的教程中会默认提供方法一的命令。

Nonebot提供了三种管理插件的方法，请确保你的终端当前处于项目文件夹中。
其中使用`pip`安装（方法三）需要手动激活Python虚拟环境，方法一、二管理插件时会自动使用虚拟环境。
### 5.1.1.方法一：命令行管理
在终端输入以下命令：
```
nb plugin install <插件名称>  # 安装插件
nb plugin uninstall <插件名称>  # 卸载插件
nb plugin update <插件名称>  # 升级插件
nb plugin list  # 列出商店所有插件
nb plugin search [关键词]  # 搜索商店插件
```
### 5.1.2.方法二：交互式管理
在终端输入以下命令：
```
nb plugin
```
将进入控制台交互模式，键盘上下键移动光标，空格选中，回车确认。
![Plugin1](http://source.cclmsy.cc/Images/BOT/BOT_21.png)
按照提示输入插件名称即可。
### 5.1.3.方法三：使用pip安装
如果你在第3节中正确创建了项目，那么你的项目文件夹中会有一个`.venv`文件夹，这是项目的Python虚拟环境文件夹。

值得一提的是，虚拟环境是Python提供的一个非常实用的功能，它可以将项目环境和全局环境隔离。
你可以在虚拟环境中安装和管理项目特定的Python库和依赖项，而不会与全局Python环境相互影响。

在终端输入以下命令以激活虚拟环境：
```
.\.venv\Scripts\activate
```
如果遇到“禁止运行脚本”的报错，可以参考我的这篇文章来解决：[Windows系统无法激活Python虚拟环境的解决方案](/posts/28dedfa6.html)
如果命令行开头出现一个括号，内容为你的项目名称，则代表Python虚拟环境已经成功激活。

接下来你可以使用pip命令来管理你的插件：
```
pip install <插件名称>  # 安装插件
pip uninstall <插件名称>  # 卸载插件
pip install --upgrade <插件名称>  # 升级插件
pip list  # 列出当前已经安装的插件（包含Nonebot依赖包）
```
`pip`相比前两种方法，缺少了列出和搜索商店插件的功能，多了可以查询当前已经安装了的插件的功能。
在忘记已经安装的插件或插件名情况下查询比较方便（当然你也可以自己在`.\.venv\Lib\site-packages`里查看）。

## 5.2.安装go-cqhttp
`go-cqhttp`框架提供了接收QQ事件与调用QQ的API的能力，是QQ机器人的核心组件。
NoneBot插件商店中提供了这个框架的插件，插件名称为：`nonebot-plugin-gocqhttp`
在终端中输入以下命令安装插件：
```
nb plugin install nonebot-plugin-gocqhttp
```
安装完成后，在终端中输入以下命令运行机器人：
```
nb run
```
出现以下提示则安装和运行成功：
![gocq1](http://source.cclmsy.cc/Images/BOT/BOT_23.png)
按住Ctrl单击链接进入`go-cqhttp`的WebUI界面
![gocq2](http://source.cclmsy.cc/Images/BOT/BOT_24.png)

# 6.登录QQ机器人与检查
## 6.1.登录QQ机器人
点击左上角“添加账号”，输入要部署的QQ账号，密码留空，设备类型选择“Android Watch”：
（注：也可以选择账号密码登录，但可能出现账号被风控无法登录的情况，手表协议登录相对比较稳定。）
![QQLogin1](http://source.cclmsy.cc/Images/BOT/BOT_25.png)
添加完成后点击启动，待加载出二维码后点击“显示登录二维码”。
用你的手机登录要部署的QQ账号，扫描二维码登录。
![QQLogin2](http://source.cclmsy.cc/Images/BOT/BOT_26.png)
登陆完成后会有消息提示如下：
![QQLogin3](http://source.cclmsy.cc/Images/BOT/BOT_27.png)

## 6.2.检查QQ机器人是否正常运行
登录你自己的QQ账号，添加机器人为QQ好友，然后按以下格式发送给机器人消息：
```
\echo [自定义消息]
```
请注意，不论你有没有配置命令起始字符，在这里echo的开头**必须包含"\\"字符**。
如果机器人回复了和自定义消息相同的内容，那么恭喜你，你已经成功地完成了QQ机器人的部署。
{% hideBlock 点我预览示例, --theme-color %}
<img src="http://source.cclmsy.cc/Images/BOT/BOT_28.png" alt="image" style="zoom:67%;" />
{% endhideBlock %}

这一步完成后，建议将项目文件夹复制一份作为备份。
这样后续重置机器人时，只需要将备份复制一份回来即可。

# 7.为你的QQ机器人赋能
完成了QQ机器人部署后，你就可以为你的机器人添加插件以实现各种各样的功能。插件的管理方式见`5.1`节。
你可以在官方插件商店找到你需要的插件，根据插件开发者提供的的文档进行安装与配置：[插件商店](https://nonebot.dev/store/plugins)。
如果你有足够的能力和精力，可以学习NoneBot文档，编写开发自己的插件：[NoneBot文档](https://nonebot.dev/docs/)。

# 8.注意事项
1. 机器人的运行进程由终端调起，在你运行机器人的过程中，请务必保证你原来的终端窗口一直保持开启状态。如果你使用的是VSCode之类的IDE，请不要关闭你终端所在的窗口。
2. 机器人以你的计算机作为服务器端，BOT使用期间请务必保证你的服务器/计算机处于开机联网状态。
3. Windows系统时常会进行自动更新，必要时可以关闭自动更新。