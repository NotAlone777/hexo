---
title: 解决Pyinstaller无法安装问题
tags:
  - Python
categories:
  - Python
date: 2021-05-16 23:19:21
data: 2019-11-14 18:34:13
summary: 使用setup.py安装Pyinstaller
---

今天用传统的`pip`安装`Pyinstaller`竟然翻车了，既然用`pip`不能够正常安装，那就采用其它的安装方法，本文采用`setup.py`来安装`Pyinstaller`

- 1.先去下载`Pyinstaller`的安装文件，下载地址如下<http://www.pyinstaller.org/downloads.html>下载如图黄色框中的那个压缩包。
![Pyinstall](https://od.alonesoul.club/api?path=/Blog/20210516/Pyinstaller%E5%AE%89%E8%A3%85%E5%8C%85.jpg&raw=true)
- 2.下载完后解压，然后打开`cmd`终端，进入到解压后的文件夹中，运行下面这条命令安装
    ```bash
    python setup.py install
    ```
- 3.等待安装结束即可。
