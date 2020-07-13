---
title: Hexo中本地图片正确显示方法
tag: Hexo
date: 2020-5-26
---

## 插件安装与配置

安装一个图片路径转换的插件`hexo-asset-image`

```
npm install hexo-asset-image --save
```

修改`/node_modules/hexo-asset-image/index.js`，代码更换为以下代码

```js
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
    	var link = data.permalink;
	if(version.length > 0 && Number(version[0]) == 3)
	   var beginPos = getPosition(link, '/', 1) + 1;
	else
	   var beginPos = getPosition(link, '/', 3) + 1;
	// In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
	var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
		if ($(this).attr('src')){
			// For windows style path, we replace '\' to '/'.
			var src = $(this).attr('src').replace('\\', '/');
			if(!/http[s]*.*|\/\/.*/.test(src) &&
			   !/^\s*\//.test(src)) {
			  // For "about" page, the first part of "src" can't be removed.
			  // In addition, to support multi-level local directory.
			  var linkArray = link.split('/').filter(function(elem){
				return elem != '';
			  });
			  var srcArray = src.split('/').filter(function(elem){
				return elem != '' && elem != '.';
			  });
			  if(srcArray.length > 1)
				srcArray.shift();
			  src = srcArray.join('/');
			  $(this).attr('src', config.root + link + src);
			  console.info&&console.info("update link as:-->"+config.root + link + src);
			}
		}else{
			console.info&&console.info("no src attr, skipped...");
			console.info&&console.info($(this));
		}
      });
      data[key] = $.html();
    }
  }
});

```

 <!-- more -->

打开`_config.yml`文件，修改下述内容

```yml
post_asset_folder: true
```

## 本地操作

在`_posts`中创建与文章名同名的文件夹，文件夹中放入文章中需要插入的图片

<center><img src="Hexo中本地图片正确显示方法\image.png" alt="image"  /></center>

插入图片方式一（不能方便缩放图片和居中图片且不能在markdown编辑器中预览）

```markdown
![image](image.png)
```

插入图片方式二（推荐）

```html
<img src="Hexo中本地图片正确显示方法\image.png" alt="image"  />
```

```html
<center><img src="Hexo中本地图片正确显示方法\image.png" alt="image" style="zoom:80%;" /> </center>
```

