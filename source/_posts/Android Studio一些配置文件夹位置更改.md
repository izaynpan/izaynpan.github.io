---
title: Android Studio一些配置文件夹位置更改
tag: others
date: 2022-3-5
---

Android Studio默认在系统盘里创建安卓虚拟机、下载gradle相关文件，而AndroidStudio本体和Sdk位置可在安装时更改。以上文件皆会吃掉大量硬盘空间，可更改到系统盘外其他硬盘。

<center><img src="Android Studio一些配置文件夹位置更改\img1.jpg" /></center>

## 更改.gradle位置

gradle相关文件默认在`系统盘:\Users\username\.gradle`（仅针对Windows），关闭Android Studio后将文件夹.gradle整个剪切到新路径，再打开Android Studio，在Settings里搜索gradle更改Gradle user home的位置为新路径即可。

<center><img src="Android Studio一些配置文件夹位置更改\img2.jpg" /></center>

## 更改安卓虚拟机位置

安卓虚拟机位置默认在`系统盘:\Users\username\.android\avd`（仅针对Windows），关闭Android Studio后将文件夹avd整个剪切到新路径，再新增一个系统环境变量，变量名为`ANDROID_AVD_HOME`，变量值为新路径，再次打开Android Studio创建新虚拟机时会默认创建到新路径下。若avd里已有虚拟机，将.ini文件里的path改为新路径即可。

<center><img src="Android Studio一些配置文件夹位置更改\img3.jpg" /></center>
