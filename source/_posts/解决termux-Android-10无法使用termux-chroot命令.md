---
title: 解决termux Android 10无法使用termux-chroot命令
date: 2021-05-16 23:51:55
tags:
  - Android
  - Termux
categories: Android
summary: 修复在Android 10以上版本termux-chroot无法使用的问题
---
这几天心血来潮去玩`termux`，结果最难受的是突然发现`termux-chroot`命令无法使用，经过在茫茫网络中的搜索，终于在GitHub上找到了解决方案。

参考这个问题，大佬们的答案
[https://github.com/termux/proot/issues/87](https://github.com/termux/proot/issues/87)

发现修改termux-chroot的脚本后可以完美运行，废话不多说进入正题

### 修改termux-chroot脚本
```bash
vi /data/data/com.termux/files/usr/bin/termux-chroot
```
插入红框里的这几行代码
![](https://od.alonesoul.club/api?path=/Blog/20210516/web.png&raw=true)

```
# Android 10 needs /apex for /system/bin/linker:
# https://github.com/termux/proot/issues/95#issuecomment-584779998
if [ -d /apex ]; then
	ARGS="$ARGS -b /apex:/apex"
fi
```
输入`:wq `保存退出
接下来就可以完美的使用`termux-chroot`命令了。
![效果展示](https://od.alonesoul.club/api?path=/Blog/20210516/termux.png&raw=true)