---
title: Windows10桌面右键菜单添加深色模式与亮色模式
tag: win10
date: 2020-5-29
---

新建`桌面右键应用模式.reg`，添加以下代码保存，为防止乱码，以ANSI编码保存，双击运行写入注册表。

*PS: 此种方式修改主题模式不适用于任务栏*

```
[HKEY_CLASSES_ROOT\DesktopBackground\Shell\AppMode]

"icon"="themecpl.dll,-1"

"MUIVerb"="应用模式"

"Position"="Bottom"

"SubCommands"=""

[HKEY_CLASSES_ROOT\DesktopBackground\Shell\AppMode\shell]

[HKEY_CLASSES_ROOT\DesktopBackground\Shell\AppMode\shell\001flyout]

"Icon"="imageres.dll,-5411"

"MUIVerb"="亮色模式"

[HKEY_CLASSES_ROOT\DesktopBackground\Shell\AppMode\shell\001flyout\command]

@="Reg Add HKCU\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Themes\\Personalize /v AppsUseLightTheme /t REG_DWORD /d 1 /f"

[HKEY_CLASSES_ROOT\DesktopBackground\Shell\AppMode\shell\002flyout]

"Icon"="imageres.dll,-5412"

"MUIVerb"="深色模式"

[HKEY_CLASSES_ROOT\DesktopBackground\Shell\AppMode\shell\002flyout\command]

@="Reg Add HKCU\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Themes\\Personalize /v AppsUseLightTheme /t REG_DWORD /d 0 /f"
```

