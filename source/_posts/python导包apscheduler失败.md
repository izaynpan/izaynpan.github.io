---
title: python导包apscheduler失败
tag: python
date: 2022-10-24
---

运行nonebot的一个语音插件报错`Failed to import "nonebot_plugin_apscheduler"`，最后定位到`ZoneInfoNotFoundError`，即时区的问题，我上设置瞧了一眼时区是东八区，按理说应该是正确的，不清楚为什么获取不到，最后的解决方法是设置成**自动获取时区**。其他apscheduler类的时区报错也可参考。

