---
title: Hexo博客备份与恢复
tag: Hexo
date: 2020-7-13
---

## 思路

在仓库里创建新的分支，存放Hexo生成的原始文件，原master分支存放生成的静态网页。

## 备份

博客内文件或文件夹的内容：

- `.deploy_git`: 执行`hexo g`自动生成，不需拷贝
- `node_modules`: 安装包的目录，执行`npm install`自动生成，不需拷贝
- `public`: 执行`hexo g`自动生成，不需拷贝
- `scaffolds`: 文章的模板，**需拷贝**
- `source`: 包含生成网页需要的源文件和其他资源文件，**需拷贝**
- `themes`: 主题，**需拷贝**
- `.gitignore`: 在push时忽略的文件，**需拷贝**
- `_config.yml`: 站点的配置文件，**需拷贝**
- `db.json`: 文件，不需拷贝
- `package.json`: 依赖的模块列表，**需拷贝**
- `package-lock.json`: 模块安装记录，自动生成，不需拷贝

