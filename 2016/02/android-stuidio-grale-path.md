Title: .AndroidStudio1.x .android .gradle位置问题
Date: 2016-02-23 23:20
Tags: Android Studio, gradle, android, Home
Category: 备忘

**问题描述： Android Studio 启动模拟器提示 Home 已配置但未在 Home 下找到 xxx.ini，不能启动模拟器**


**说明：**

Android Studio 和 gradle 以及 android sdk 支持通过环境变量( Androidstudio 是启动参数)改变对应生成的 .xxxx 的位置，
先检查发现并未专门配置对应的环境变量，
又检查发现在 Home 和另外一处目录下都有 .androidStudio1.x .android .gradle 这几个文件夹，
这个就比较奇怪。


最后查找知晓
[Android Studio 在 windows 上启动时会根据 **桌面文件夹** 的位置来创建 .androidStudio1.x .android 和 .gradle 文件夹](http://stackoverflow.com/a/34701807/4955673)，
而 android sdk 和 gradle 默认会在 Home 目录下生成 .android 和 .gradle 文件夹, 这两个文件夹与 Android Studio 不同时就会引起程序逻辑混乱。


本次问题即因为生成的 avd 在 .androidStudio1.x 同级的 .android 下，
而启动时却在 Home 目录下的 .android 文件夹内寻找(之前更改过我的文档，视频，图片，收藏等位置时手欠更改了桌面的位置……)，
结果找不到引起的问题。
