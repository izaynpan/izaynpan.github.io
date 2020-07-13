---
title: Windows10删除启动项
tag: win10
date: 2020-5-25
---

### 方法一：利用微软工具包Sysinternals Suite下的Autoruns工具

Autoruns运行后即会自动扫描系统当前的启动项，扫描结果列表中粉红色的项目表示该条目对应的应用没有数字签名、签名不匹配或没有发行商信息；显示为黄色的项目表示该启动项对应的文件已经不存在了。

所以黄色项可以直接删除，粉红色项斟酌一下，如不安全也可删除。删除方法很简单，在某个启动项上点击右键，选择“删除”即可。

### 方法二：在注册表中删除启动项

启动项在注册表中的路径为：

> HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

HKCU实际上就是HKEY_CURRENT_USER的缩写。因此实际需要定位的路径就是：

> HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

删除其下的相关启动项值即可。

另外有些启动项目可能在如下路径：

> *HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run*

*PS：想快速定位到该路径，可以右键一键转到同名项。*