---
title: 更改Spotify Premium win端的默认缓存位置
tag: win10
date: 2020-6-21
---

Spotify虽然设置里提供了更改缓存位置的功能，但Premium版还是会保存在默认的c盘下`C:\Users\用户名\AppData\Local\Spotify\Data`（Premium用户感觉有被冒犯到），重度用户可能一个月就攒了8G(＃°Д°)，所以应该很有必要解决一下（1T的SSD做系统盘请忽视）。

1．选好缓存的新家，比如`D:\Spotify\Data`，将原缓存全部搬到新家并对旧家进行拆迁操作(ﾉ*･ω･)ﾉ

2．打开万能的cmd，输入`mklink /J "C:\Users\用户名\AppData\Local\Spotify\Data" "D:\spotify\Data"`，成功会提示`Junction created for C:\Users\用户名\AppData\Local\Spotify\Data <<===>> D:\spotify\Data`之类的巴拉巴拉。

感觉此时两文件夹就像哆啦A梦的两个四次元口袋，连接着同一个地方，不过本体在新家。

是不是可以用这种方法把c盘一些很大的文件全部移走？（滑稽被打）

Windows下mklink命令参考[博文1](https://liam.page/2018/12/10/mklink-in-Windows/)。

2022-8-8加注：今天复习到操作系统的文件共享知识，瞬间联想到mklink指令，mklink /H即基于索引结点的共享方式（硬链接），mklink /D(/J?) 即基于符号键实现文件共享（软链接）。至于mklink /D和mklink /J的区别见[博文2](https://zhuanlan.zhihu.com/p/98101500)，[博文3](https://blog.csdn.net/NotBack/article/details/73604292)。

2022-10-25加注：发现个软件`FolderMove`能直接实现这个功能，几百KB的大小，无需安装。[下载地址](https://foldermove.com/)
