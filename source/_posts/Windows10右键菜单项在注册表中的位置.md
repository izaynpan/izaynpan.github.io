---
title: Windows10右键菜单项在注册表中的位置
tag: win10
date: 2020-6-28
---

`win + R : regedit`打开注册表编辑器（家庭版的小伙伴自行想办法解决，小声：垃圾微软），右键菜单所在注册表的位置都在`HKEY_CLASSES_ROOT`里。

- **文件右键：**`HKEY_CLASSES_ROOT\*\shell`, `HKEY_CLASSES_ROOT\*\shellex\ContextMenuHandlers`

- **文件夹右键：**`HKEY_CLASSES_ROOT\Directory\shell`, `HKEY_CLASSES_ROOT\Directory\shellex\ContextMenuHandlers`
- **文件夹中空白处右键：**`HKEY_CLASSES_ROOT\Directory\Background\shell`, `HKEY_CLASSES_ROOT\Directory\Background\shellex\ContextMenuHandlers`
- **桌面右键：**`HKEY_CLASSES_ROOT\DesktopBackground\Shell`, `HKEY_CLASSES_ROOT\DesktopBackground\shellex\ContextMenuHandlers`

知道位置后就可以魔改了( •̀ ω •́ )✧，删除多余的或者添加需要的功能。（**修改有风险，操作需谨慎**）

PS：[RightMenuMgr](https://cdn.libertycola.workers.dev/search?source=hp&ei=XdQPX8TKMZnhz7sPvqGFmAs&q=RightMenuMgr&oq=RightMenuMgr&gs_lcp=CgZwc3ktYWIQAzICCAAyAggAMgIIADIECAAQHjIECAAQHlDBIljBImDINWgBcAB4AIABpgGIAaYBkgEDMC4xmAEAoAECoAEBqgEHZ3dzLXdperABAA&sclient=psy-ab&ved=0ahUKEwiE7dHR9NDqAhWZ8HMBHb5QAbMQ4dUDCAc&uact=5)真香（滑稽被打）

