---
title: flv/mkv格式无损转mp4
tag: 多媒体
date: 2020-6-16
---

使用[MediaCoder](https://www.mediacoderhq.com/)，原理是提取原格式的视频和音频，然后用h.264标准封装成mp4格式。

底下“视频”一栏勾选“复制视频流”，“音频”一栏勾选“复制音频流”，目的是不压缩视频和音频，直接以原码率封装，然后“START”。

或者用FLVExtract/MKVExtractGUI2提取，然后用MeGUI封装。

注：mp4不支持封装内挂字幕，所以原格式封装的内挂字幕会消失。（剪刀手的福音不是吗XD）

