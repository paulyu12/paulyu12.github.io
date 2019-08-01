---
layout:     post
title:      "RobotFramework+Appium自动化APP测试"
# subtitle:   ""
date:       2019-07-23
author:     "Paul"
header-img: "img/post-bg-2019.jpg"
tags:
    - 技术
---
接到测试内部开发的APP的任务，需要在Android和iOS两个平台进行测试，并且对于iOS的不同版本、Android中不同手机厂商的定制版本进行充分的测试。为了自动化测试过程，采用RobotFramwork+Appium对APP进行测试。由于目前设备受限（没有Macbook），所以本文以介绍Android自动化测试为主。

### 一. 原理

利用Appium软件在本地搭建Appium服务器（也可以搭建在远程主机上）。测试脚本由Robot Framework（RF)生成。RF通过socket将命令发送给Appium服务器（一般是4723端口），也从Appium服务器得到测试结果。

Appium服务器相当于代理，会在移动设备上安装一个APP——Appium Settings作为对端，通过adb工具（由Android SDK提供）和socket将命令发送给移动设备，操纵设备进行自动化测试，并获取测试结果。

在iOS测试中，Appium和移动设备间的通信方式使用Instruments。

### 二. Robot Framework （RF)

Robot Framework 是一款基于 Python 的功能自动化测试框架。它具备良好的可扩展性，支持关键字驱动，可以同时测试多种类型的客户端或者接口，可以进行分布式测试执行。主要用于轮次很多的验收测试和验收测试驱动开发（ATDD）。

### 三. Appium

Appium在本机或远程机器建立服务，测试人员在本地客户端编写测试脚本，将批量的测试任务发送给Appium服务，由Appium负责将命令发送给运行App的设备或模拟器，返回的结果向本地客户端报告。由此实现了自动化App测试。

### 四. 环境搭建和测试流程

1. 安装 RobotFramework
  
   - [IBM安装教程](https://www.ibm.com/developerworks/cn/opensource/os-cn-robot-framework/index.html)
   
2. 安装 Appium

   - 如果是 windows 10 系统，则需要用管理员权限启动 Appium（否则会报错，具体问题不记得了）.
   
3. 安装 Android模拟器（本实验使用夜神模拟器）
   - 打开夜神模拟器的”开发人员调试功能“
   - 保证 Android SDK 安装目录下的 adb.exe 版本和模拟器安装目录下的 nox_adb.exe 版本一致。
   
4. 安装 JDK & Android SDK

   - JDK 需要安装版本为 8.*，否则会出现问题（具体问题不记得了...，我最初安装的 v12）
   - 在模拟器已经打开 USB 调试模式的情况下，如果 Android SDK 中 adb.exe 的版本与夜神模拟器中的 nox_adb.exe 版本不一致，则用 adb 命令打印所有已连接设备列表，会找不到该设备。这时，需要用 Android SDK 中的 adb.exe 副本替换 nox_adb.exe，则会解决该问题。
   - 安装完 JDK & Android SDK 后需要将 bin 等相关目录添加到系统环境变量中。
   - adb devices 查看已通过 USB 连接到本地的设备，如果模拟器此时是开启状态，在列表中应该出现。
   
5. 安装 node.js

6. 使用 RobotFramework 脚本 ride.py 书写测试用例
  
   - ride.py 脚本位于 Python 的安装目录下的子目录：<Python 根目录>/Scripts/ride.py
   - 新建 project
   
      - File -> New Project
      - Type 里面选择 Directory
      - 设置 Project 名字，假设为 test-project
   - 新建 suite
   
      - 在侧边栏 Project 名字上右键鼠标
   - New Suite
   - 新建 test usecase
   
      - 在侧边栏 Suite 名字上右键鼠标
      - New test usecase
- 为 suite 导入 Library 和 Resource (如有)
  
      - 对于测试移动 App，需要导入 AppiumLibrary；对于 Web 测试，一般需要导入Selenium2Library
      
      - 可以自定义封装 AppiumLibary 中的步骤流，可以以 Resource 形式导入到 Suite 中，形成一个新的“函数”（这里的函数不是严格意义上的高级程序设计语言中的函数，函数名可以用中文命名）。在 编写 test usecase 时调用。
      
      - 引用 Library 或 Resource 后，如果该包名显示为红色，则说明发生引用错误。可以通过查看日志，跟踪 python 脚本的报错来解决。
      
      - 在 ride GUI 编辑区输入一条函数后，如果该函数名字变为蓝色，则说明该函数是一个被定义过的函数且成功调用。
   - 在编写测试用例脚本时，需要用到Library提供的各种“函数”，在 ride.py 的图形化界面中用快捷键 F5 能够快速调出一个关键字搜索程序，能够找到我们导入的 Library 都支持哪些函数。
   - 以函数“Open Application”为例，需要几个参数：其中需要 Android App 的包名（Package) 和主活动名 （main Activity）。可以通过 Android SDK 提供的工具 aapt.exe（在 Android SDK 的安装目录下的 bin 子目录）: 将 aapt 程序所在目录加入到系统环境变量中，然后打开 cmd，运行命令
      `# aapt dump badging <安卓apk包名所在路径/xxx.apk>`
   其中 package: name 就是包名，Launchable-activity 就是主活动（Main activity/）名。
  
7. uiautomatorviewer.bat
   - 这个批处理程序是一个能够抓取当前设备显示的 Android App 页面的控件的工具。
   - 左上角有两个类似手机的按钮，按其中一个如果抓取不到则按另一个抓取。
  
8. xpath

   - 在 robotframework 中编写用例时，需要根据页面中元素作出相应的动作（如 click, input 等），这些动作是建立在能够抓取到元素的基础上。抓取元素通常根据元素的唯一标识，xpath 定位符即是一种几乎万能的定位方式。
   - xpath定位元素的例子：//*[@text='确定']，这里是在页面中定位一个元素，其 text 字段为“确定”；如果定位到多个 text 字段相同的元素，则可以用列表形式索引。一般建议使用能够唯一标识的字段进行定位。
   - 我一般是用元素 resource-id、text、class 等字段来搜索元素的。
   - 了解更多 xpath 定位语法的使用，请使用搜索引擎。
