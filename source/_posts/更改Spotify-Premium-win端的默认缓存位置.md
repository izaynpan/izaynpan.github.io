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

Windows下mklink命令参考[博文](https://liam.page/2018/12/10/mklink-in-Windows/)。

