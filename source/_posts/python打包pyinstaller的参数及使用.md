---
title: python打包pyinstaller的参数及使用
date: 2019-11-14 19:23:04
categories: 
- Python
tags:
- Python
- Windows
summary: 打包Python程序为.exe文件的基本方法
---
由于经常要打包python的写的程序为`.exe`应用程序，写下来备忘。

#### 1.安装pyinstaller
 ```python
 pip install pyinstaller
 ```

- Tip：若安装失败可以试试下面这种方法
 <https://alonesoul.club/2021/05/16/jie-jue-pyinstaller-wu-fa-an-zhuang-wen-ti/>

#### 2.常用的打包命令
在`cmd`里进入到要打包的程序目录，然后执行如下命令
 ```python
 pyinstaller -w  xxx.py
 ```
等待出现`successful`就打包成功了，打包后的程序会在当前目录的`dist`文件夹里，接着把要需要的其它文件复制进去就行了。

#### 3.pyinstaller的打包参数
|参数| 说明 |
|--|--|
|-F, –onefile |把python程序打包为一个可执行文件（一个单独的.exe文件）,不大建议使用  |
|-D, –onedir|产生一个目录（包含多个文件）作为可执行程序|
|-w，--windowed，--noconsolc|程序运行时不显示命令行窗口（仅对 Windows 有效）|
|-c，--nowindowed，--console|指定使用命令行窗口运行程序（仅对 Windows 有效）|
|-d，--debug|产生 debug 版本的可执行文件|
|-a，--ascii|不包含 Unicode 字符集支持|
|-o DIR，--out=DIR|指定 spec 文件的生成目录。如果没有指定，则默认使用当前目录来生成 spec 文件|
|-p DIR，--path=DIR|设置 Python 导入模块的路径（和设置 PYTHONPATH 环境变量的作用相似）。也可使用路径分隔符（Windows 使用分号，Linux 使用冒号）来分隔多个路径|
|-n NAME，--name=NAME|指定项目（产生的 spec）名字。如果省略该选项，那么第一个脚本的主文件名将作为 spec 的名字|
|–icon=<FILE.ICO>，-i <FILE.ICO>|将file.ico添加为可执行文件的资源(只对Windows系统有效)，改变程序的图标  用法 : pyinstaller -i  ico路径 xxx.py|
|-add-data|打包额外的资源 用法 : pyinstaller xxx.py -add-data=src;dest。windows以;分割，Linux以:分割|
|-add-binary|打包额外的代码。用法和上面和-add-data一样，区别在于，用binary添加的文件，pyinstaller会分析它引用的文件并把它们一块添加进来|
|--key|pyinstaller会储存字节码，指定加密字节码的key（16位的字符串）（Windows特有）|
|--version-file|添加版本信息文件 用法 : pyinstaller -version-file version.txt（Windows特有）|
|-m, -manifest|添加manifest文件 用法 : pyinstaller -m main.manifest（Windows特有）|