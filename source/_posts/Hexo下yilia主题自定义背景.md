---
title: Hexo下yilia主题自定义背景
tag: Hexo
date: 2020-5-30
---

在`themes/yilia/layout/layout.ejs`的body标签内加入以下代码：

```html
<body  style="background: url(/assets/img/background.png); background-size: cover;background-repeat: no-repeat;opacity: 0.85"></body>
```

```
background: url();//路径，可以直接插入网上的图片或者插入本地图片
/assets/img/background.png;//位置为Hexo根目录下的assets/img
background-size: cover;//表示图片根据屏幕大小自适应占满整个屏
background-repeat: no-repeat;//表示不平铺
opacity: 0.85;//透明度
```

