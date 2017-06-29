---
title: FFmpeg命令行工具使用
date: 2016-11-25 15:09:05
tags: video
---

> 来自[雷神的blog](http://blog.csdn.net/leixiaohua1020)

### 基本DOS命令
* 切换到上一级文件夹 cd ..
* 切换到当前目录下名为xxx的文件夹 cd xxx
* 改变当前盘符命令 c:
* 查看目录内容命令 dir
* 创建文件夹命令 mkdir xxx
* 文件复制命令 copy
* 删除文件命令 del
* 清楚屏幕命令 cls
* 查看ip ipconfig

### FFmepg
* 下载
 * 访问FFmpeg官网（ http://ffmpeg.org） →选择Download→选择Windows Package→进入Zeranoe FFmpeg网站。
 * 注意不要直接从FFmpeg官网下载源代码。
* 版本说明
 * Zeranoe网站中的FFmpeg分为3个版本：
 1. Static：只包含3个体积很大的exe。
 2. Shared：除了3个体积较小的exe之外，还包含了dll动态库文件。
 3. Dev：只包含了开发用的头文件（ *.h）和导入库文件（ *.lib）。
**PS: 命令行使用的时候下载Static或者Shared版本就可以了。**
* 使用
 * 将下载下来的压缩包解压到任意目录。
 * 打开命令行工具，切换到ffmpeg的bin文件夹。
 * 命令行输入ffmpeg.exe，查看弹出信息。
![png1](https://github.com/865077695/img/blob/master/ffmpeg_DOS/1.png?raw=true)
![png2](https://github.com/865077695/img/blob/master/ffmpeg_DOS/2.png?raw=true)

### FFmpeg.exe使用
* 命令格式
 * 功能
 ffmpeg.exe用于视频的转码。
 * 最简单的命令
 `ffmpeg -i input.avi -b:v 640k output.ts`
 该命令将当前文件夹下的input.avi文件转换为output.ts文件，并将output.ts文件视频的码率设置为640kbps。
 * 命令格式
 `ffmpeg -i {输入文件路径} -b:v {输出视频码率} {输出文件路径}`
 所有的参数都是以键值对的形式指定的。例如输入文件参数是“ -i”，而参数值是文件路径；输出视频码率参数是“ -b:v”，而参数值是视频的码率值。但是注意位于最后面的输出文件路径前面不包含参数名称。
 * 命令参数
![png3](https://github.com/865077695/img/blob/master/ffmpeg_DOS/3.png?raw=true)
**PS：详细的参数可以访问[http://ffmpeg.org/ffmpeg.html](http://ffmpeg.org/ffmpeg.html)**

### ffplay.exe的使用
* 命令格式
 * 功能
 ffplay.exe用于视频播放
 * 最简单的命令
 `ffplay input.avi`
 该命令将播放当前文件夹下的input.avi文件
 * 命令格式
 `ffplay {输入文件路径}`
 ffplay.exe的参数格式和ffmpeg.exe是类似的。所有的参数都是以键值对的形式指定的（由于不包含输出文件，所以只能指定输入参数）。注意位于最后面的输入文件路径前面不包含参数名称。
 * 快捷键
![png4](https://github.com/865077695/img/blob/master/ffmpeg_DOS/4.png?raw=true)
**PS：详细的参数可以访问http://ffmpeg.org/ffplay.html**

### 练习
* 使用ffmpeg.exe将测试文件转换成如下格式
 * MKV文件，起始时间为第10s, 持续时间为10s。
 * MP4文件，视频编码器为libx264，音频编码器为libmp3lame。
 * AVI文件，分辨率为640x360，帧率为25fps，视频码率为600kbps，音频码率为64kbps。
 * FLV文件，视频编码器为libx264，音频编码器为libmp3lame，音频采样率为44100Hz。
 * YUV420P格式像素数据，分辨率为640x360。
 * H.264码流文件，分辨率为640x360。
 * WAVE格式音频采样数据。
 * MP3码流文件。
