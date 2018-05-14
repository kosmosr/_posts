---
title: 【记录】Mac下编译Redis Desktop Manager
date: 2018-02-26 15:44:02
categories:
- Mac
- Redis
---
## 【记录】Mac下编译Redis Desktop Manager



#### 说明：

首先根据官网的[编译教程](http://docs.redisdesktop.com/en/latest/install/#build-from-source) 发现蛮多需要注意的地方 花了几个小时折腾。。 所以记录下

首先需要安装Xcode 和 Command Line Tools 一般下最新的稳定版就行了 推荐去这里[下载](https://developer.apple.com/download/more/)，版本注意统一下。

然后安装下[Homebrew](https://brew.sh/) 这也是mac下必备的工具了 没什么说的。



#### 下载源码：

注意下分支版本 本文章用的是当前最新的

`git clone --recursive https://github.com/uglide/RedisDesktopManager.git -b 0.9 rdm`

然后cd 到rdm/src目录下，执行命令：

`./configure`

> 注意如果这里报错error: tool 'xcodebuild' requires Xcode，需要使用命令指定下Xcode 如果有多个Xcode 视自己情况
>
> 终端命令：
>
> xcode-select --switch /Applications/Xcode.app/Contents/Developer/
>



#### QT安装部分：

​	这里是比较坑的地方 官网的教程说的很简单 花了好多时间在这里

​	首先先去官网下载open source的版本，进入到安装界面


[![1.jpg](http://www.wailian.work/images/2018/02/26/1.jpg)](http://www.wailian.work/image/MUakgO)

​	到这个界面：

![2.jpg](http://www.wailian.work/images/2018/02/26/2.jpg)

​	按我这样选择就行了 QT版本至少**5.9**以上 **Qt Charts**组件必须选择 然后继续下一步等待下载安装。



#### 编译：

​	打开Qt creator，然后打开刚刚下载源码中的src/rdm.pro，在左侧的编辑界面，注释掉图中的app_budle，

​	这样将会Qt编译完成的时候生成一个app。

![4ddf94.jpg](http://www.wailian.work/images/2018/02/26/4ddf94.jpg)

​	然后进入项目界面，构建配置选择Release，然后选择下面的绿三角运行
![5.jpg](http://www.wailian.work/images/2018/02/26/5.jpg)

​	这时候发现两个问题：
[![6.jpg](http://www.wailian.work/images/2018/02/26/6.jpg)](http://www.wailian.work/image/MU2PF6)
​	 花了点时间研究了下，这里我直接给出解决方案：

 在rdm目录下的bin/Frameworks/Crash Reporter.app右键显示包内容，把里面的crash_report_sender放到bin/osx/debug/下并重命名为crashreporter，然后继续在src/resources下 把Info.plist.sample命名为Info.plist，这两个报错就解决了，再一次执行编译。


 这个时候就可以在..bin/osx/debug/下看到编译出来的app文件了，但是这个文件只能在本地上使用，还需要进一步打包才行。qt给我们提供了macdeployqt工具，可以让我们生成完整的app文件，

 macdeployqt工具在你的Qt安装目录下/clang_64/bin/路径下，最好把它加入环境变量或者用alias里，方便在终端下使用

[![8.jpg](http://www.wailian.work/images/2018/02/26/8.jpg)](http://www.wailian.work/image/MU2oqD)

 然后回到刚刚生成的app所在目录下，在终端下执行指令：

   `macdeployqt rdm.app -qmldir=../../../src/qml -dmg`

   然后就打包好了，可以给别人用了！
