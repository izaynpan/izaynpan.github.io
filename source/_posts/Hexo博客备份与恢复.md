---
title: Hexo博客备份与恢复
tag: Hexo
date: 2020-7-13
---

## 思路

在仓库里创建新的分支hexo，存放Hexo生成的原始文件，原master分支存放生成的静态网页。备份时上传需备份的文件到hexo分支，恢复时拉取hexo分支到本地。

## 备份

### 博客内文件或文件夹的内容

- `.deploy_git`: 执行`hexo g`自动生成，不需拷贝
- `node_modules`: 安装包的目录，执行`npm install`自动生成，不需拷贝
- `public`: 执行`hexo g`自动生成，不需拷贝
- `scaffolds`: 文章的模板，**需拷贝**
- `source`: 包含生成网页需要的源文件和其他资源文件，**需拷贝**
- `themes`: 主题，**需拷贝**
- `.gitignore`: 在push时忽略的文件，**需拷贝**
- `_config.yml`: 站点的配置文件，**需拷贝**
- `db.json`: 配置文件，不需拷贝
- `package.json`: 依赖的模块列表，**需拷贝**
- `package-lock.json`: 模块安装记录，自动生成，不需拷贝

### 具体操作

1. 在`.gitignore`里添加不需拷贝的内容，可以添加不需拷贝的主题（例如自带主题）。
2. 删除主题文件夹下的`.git/`，否则无法push（或者新增个备份文件夹`themesbk/themename`存放主题拷贝，然后push）
3. 在blog根目录执行命令`git init`初始化本地仓库
4. 继续执行命令`git checkout -b hexo`创建hexo分支，分支名称可改
5. 执行`git remote add origin https://github.com/username/username.github.io.git`添加远程仓库
6. 依次执行命令`git add .` , `git commit -m "something"` , `git push origin hexo`
7. 需备份时只需执行第6步的命令

## 恢复

1. 安装git, nodejs
2. 执行`git clone -b hexo https://github.com/username/username.github.io.git blog`克隆至blog文件夹（名称可改），clone速度太慢可使用镜像`github.com.cnpmjs.org`或者给git设置代理
3. 在blog文件夹内执行`npm install`安装必要模块（网上部分教程依次执行`npm install hexo-cli`, `npm install`, `npm install hexo-deployer-git`，笔者不清楚是否有必要）
4. 正常地更新博客然后使用`hexo g`, `hexo s`, `hexo d`就行啦（若deploy提示失败根据提示登录即可）

感谢以下小伙伴的指路ヾ(≧▽≦*)o：[指路博客1](https://www.jianshu.com/p/57b5a384f234)，[指路博客2](https://mupceet.com/2019/09/backup-hexo-blog/)

