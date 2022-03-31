---
title: pip换源
tags:
  - Python
  - Linux
  - pip
date: 2021-05-16 18:40:51
categories:
  - Python
summary: pip换源的基本操作
cover: https://od.alonesoul.club/api?path=/Blog/hexo/python.png&raw=true
---
由于经常给别人配置python环境，把pip换源过程记录一下。

## pip的一些国内镜像

- 阿里云 https://mirrors.aliyun.com/pypi/simple/
- 中国科学技术大学 https://pypi.mirrors.ustc.edu.cn/simple/
- 豆瓣 https://pypi.douban.com/simple/
- 清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

## Windows

在Windows操作系统系统下，首先使用WIN+R组合键，输入%userprofile%，按下回车
![运行](https://od.alonesoul.club/api?path=/Blog/20210516/%E8%BF%90%E8%A1%8C.jpg&raw=true)

此时会进入用户的文件夹，在此目录下创建一个名字为 `pip`的文件夹，然后进入该文件夹，创建一个名为 `pip`的文档，并把后缀改为 `.ini`，打开 `pip.ini`在文档中写入如下内容保存即可。

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

## Linux

在Linux操作系统下，修改 `~/.pip/pip.conf`,如果该文件不存在，就创建该文件，并写入如下内容保存即可。

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```
