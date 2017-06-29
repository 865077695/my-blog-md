---
title: FFmpeg获取DirectShow设备数据（摄像头，录屏）
date: 2016-11-25 18:48:42
tags: video
---

> 最近学习video,大多记录自[雷神的blog](http://blog.csdn.net/leixiaohua1020)

本片对应windows平台
### 1.列设备
当然是要在ffmpeg下的bin目录下执行了
`ffmpeg -list_devices true -f dshow -i dummy`
命令执行后输出的结果如下（注：中文的设备会出现乱码的情况）。列表显示设备的名称很重要，输入的时候都是使用`-f dshow -i video="{设备名}"`的方式。