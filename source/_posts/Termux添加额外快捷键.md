---
title: Termux添加额外快捷键
tags:
  - Android
  - Termux
categories:
  - Termux
summary: 给Termux添加额外的快捷键，方便在手机上输入特殊符号
date: 2021-05-16 23:18:04
cover: https://od.alonesoul.club/api?path=/Blog/hexo/termux.png&raw=true
---

在手机上使用Termux时，有些符号经常使用，但是输入又不太方便，所以修改下默认的Termux的拓展键盘，添加一些常用的符号。

修改配置文件 
```bash
vi ~/.termux/termux.properties
```

输入以下，也可以输入其它的，按自己的需求输入
```bash
extra-keys = [['ESC','/','-','HOME','UP','END','PGUP'],['TAB','CTRL','ALT','LEFT','DOWN','RIGHT','PGDN']]
```
重启应用即可