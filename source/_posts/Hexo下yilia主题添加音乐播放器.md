---
title: Hexo下yilia主题添加音乐播放器
tag: Hexo
date: 2020-5-31
---

> 采用网易云音乐外链（其他音乐平台类似）（偶尔抽风）

1．到[网易云音乐网页版](https://music.163.com/)获取音乐外链。

2．打开`themes/yilia/layout/_partial/left-col.ejs`，挑选位置添加代码，比如在`header-subtitle`下面。

```html
<!-- 换行防止紧跟subtitle -->
<br />
<!-- auto为0不自动播放，为1开启自动播放 -->
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=244 height=86 
        src="//music.163.com/outchain/player?type=2&id=4900975&auto=0&height=66"></iframe>
```

3．为了方便操作，在主题的配置文件`themes/yilia/_config.yml`中添加开关：

```yaml
# 音乐
# 网易云音乐
cloudMusic: 
  enable: true
```

同时第二步中的代码修改为：

```html
<!-- 增加判断 -->
<% if(theme.cloudMusic && theme.cloudMusic.enable){ %>
    <br />
    <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=244 height=86 
            src="//music.163.com/outchain/player?type=2&id=4900975&auto=0&height=66"></iframe>
<% } %>
```

> 现已换用aplayer

1．到[一个网站](https://music.liuzhijin.cn/)获取音乐直链。

2．打开`themes/yilia/layout/_partial/left-col.ejs`，挑选位置添加代码，比如在`header-subtitle`下面。

```html
<% if(theme.aplayer && theme.aplayer.enable){ %>
    <br />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.css">
    <div id="aplayer"></div>
    <script src="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/meting@2/dist/Meting.min.js"></script>
    <script>
        const ap = new APlayer({
            container: document.getElementById('aplayer'),
            audio: [{
                name: '一人行者',
                artist: '12th&桃核',
                url: 'http://music.163.com/song/media/outer/url?id=444356294.mp3',
                cover: 'http://p2.music.126.net/eQHiLT1iQ1VtNXa6UTreNA==/109951162816845875.jpg?param=130y130'
            }]
        });
    </script>
<% } %>
```

3．为了方便操作，在主题的配置文件`themes/yilia/_config.yml`中添加开关：

```yaml
# 音乐
## Aplayer
aplayer:
  enable: true
```

