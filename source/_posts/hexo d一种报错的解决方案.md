---
title: hexo d一种报错的解决方案
tag: Hexo
date: 2021-11-15
---

这不一年多没更博客了，一更就出问题😅

<center><img src="hexo d一种报错的解决方案\img.png" /></center>

解决方案参考 https://github.com/hexojs/hexo/issues/2778 下的方法（前提是系统建立了与github的ssh连接），更换`https://github.com/izaynpan/izaynpan.github.io.git/`为`git@github.com:izaynpan/izaynpan.github.io.git` 

命令为`hexo config deploy.repository git@github.com:[yougitname]/[yougitname].github.io.git`

