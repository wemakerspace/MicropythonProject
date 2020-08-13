本项目可以让你在ESP32上驱动OLED屏幕播放你想要的视频，当然了我对这个项目的研究还停留在初级阶段，
所以我强烈建议各位使用和我一样的配置。

主控：ESP32-WROVER-B(烧录了官方的支持SPIRAM的固件)，开发板是在Micropython中文社区的网店购买的
OLED屏幕：某宝购买的128*64屏幕

在介绍本项目之前，先感谢Micropython中文社区的版主邵子阳老师对本项目的帮助！

1、原理
    利用人眼的视觉暂留效应，把一段视频切分成几千张图片，然后显示在OLED屏幕上，隔0.19s刷新一次

2、图片如何在OLED上显示？
    首先，我使用的OLED是单色屏，也就是只有黑白2色。micropython的自带库中，有一个库叫“FrameBuffer”，即帧缓冲，可以创建一个缓冲区，储存你要显示的内容。接着在ssd1306中有一个函数，叫blit，只要传入缓冲帧，就可以在屏幕中显示内容了。
    其次，图片的使用也有要求，必须使用.PBM格式的图片，且必须二值化（即变成黑白2色，不是灰度图），然后用python打开文件读取里面的数据，存入缓冲区即可。大家可以直接使用我git中提供的“BAD APPLE”进行实验，这个图片也是从另一位博主的git中找到的，我尝试了使用自己制作的二值化图片，但是显示是混乱的，目前还在研究原因。或者可以去原博主的git中查看源码：https://github.com/wgzeyu/12864playbadapple

3、两种读取帧的方式
    如果你没有TF卡，但是你家里有WIFI，那么可以使用我提供的电脑端Python脚本，读取图片数据然后发送给ESP32，我是使用电脑作为服务端开启socket，ESP32作为客户端连入。网上一篇文章使用了掌控板进行显示，地址：https://mc.dfrobot.com.cn/thread-302527-1-1.html.
    但是他使用的是ESP32作为服务端，经过测试这样会导致ESP32重启，原因不明，无论是掌控板还是开发板都是，但是反过来就能使用，希望有知道为什么的朋友可以告诉我哈
    如果你有TF卡，那么可以直接把图片放在sd卡中，直接读取，这个版本的源码我也放出来了。