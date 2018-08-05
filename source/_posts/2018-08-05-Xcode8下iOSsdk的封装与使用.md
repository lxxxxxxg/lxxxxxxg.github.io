---
title: Xcode8下iOSsdk的封装与使用
date: 2018-08-05 14:24:12
tags:
---
借鉴文章：www.2cto.com/kf/201604/501934.html

中间使用的封装UI WSLoginView来自：github.com/Zws-China/WSLoginView

首先创建一个静态库文件（本文使用Xcode8.2.1）

打开Xcode 选择iOS 下 Framework&Library 下的Cocoa Touch Static Library


创建工程
创建一个任意命名的工程，我创建的是NewSDK，然后保存工程。


目录结构
一个静态库工程由头文件和实现文件组成，这些文件将被编译为库本身。

当创建静态库工程时，Xcode会自动添加NewSDK.h和NewSDK.m。你不需要实现文件，因此右键单击NewSDK.m选择delete，将它删除到废纸篓中。




删除NewSDK
为工程添加功能 此处使用的是WSLoginView 

将你的控件从Finder中拖到Xcode下NewSDK目录下 此处需要勾选add to targets NewSDK


添加功能
在NewSDK.h中 导入WSLoginView  


导入WSLoginView
静态库分为模拟器使用和手机使用  

首先以模拟器为例 ：


未编译
当生成的静态文件.a为红色时  需要模拟器编译一下 com + B 或者点击运行一下子，才会变成黑色




编译完成
然后show in finder 将编译完成的.a文件和所有的.h 文件和资源图片文件放到一个新的目录下




目录下包含内容
然后将此文件夹放入一个iOS工程下


添加到新工程下
在需要调用的地方导入 #import"NewSDK.h",添加实现方法，就可以在模拟器下运行了




运行
真机下静态库生成

真机与模拟器下静态库的生成的区别在于此处选择的是真机还是模拟器 ，其他没有区别


合并模拟器和真机静态库

把真机下和模拟器下.a文件放在同一目录，


使用命令合并 lipo -create libNewSDK.a libNewSDKDevice.a -ouput LibNewSDKBoth.a


会在目录下生成合并后的静态库文件 LibNewSDKBoth.a


libNewSDK.a: 生成的模拟器.a文件

libNewSDKDevice.a:生成的真机.a文件

libNewSDKBoth.a：合并后的结果

使用命令 lipo -info libNewSDKBoth.a  可查看结果  如果同时支持 armv7  x86_64 代表合并成功