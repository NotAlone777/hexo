---
title: 解决conda中pip无法安装包到指定虚拟环境
date: 2021-09-25 15:44:54
tags:
  - Python
  - Conda
categories:
  - Python
summary: 解决在conda其它虚拟环境中使用pip安装包却安装到base环境中
cover: https://od.alonesoul.club/api?path=/Blog/hexo/conda.png&raw=true
---
## Windows

### 1.先找到conda的安装目录

我的安装目录在 `D:\software\Miniconda3`，conda创建的其它环境默认都存在 `envs`文件夹中，如图 `d2l`文件夹就是我创建的环境
![conda其它环境所在目录](https://od.alonesoul.club/api?path=/Blog/20210925/conda%E5%85%B6%E5%AE%83%E7%8E%AF%E5%A2%83%E6%89%80%E5%9C%A8%E7%9B%AE%E5%BD%95.jpg&raw=true)

### 2.找到site.py文件

以我的目录为例，依次进入 `/d2l/Lib`即可找到 `site.py`文件
![site.py所在位置](https://od.alonesoul.club/api?path=/Blog/20210925/site.jpg&raw=true)

### 3.修改site.py文件

打开 `site.py`，找到图中的位置，并修改红框中的两个变量为你的 `conda环境的目录`
![默认变量](https://od.alonesoul.club/api?path=/Blog/20210925/%E9%BB%98%E8%AE%A4%E5%8F%98%E9%87%8F.jpg&raw=true)
![修改后的变量](https://od.alonesoul.club/api?path=/Blog/20210925/%E4%BF%AE%E6%94%B9%E5%90%8E%E7%9A%84%E5%8F%98%E9%87%8F.jpg&raw=true)

### 4.检查pip命令的位置

按下键盘 `Win`+`R`打开运行，输入 `Powershell`打开终端，并激活conda环境，激活后使用 `Get-Command pip`命令来查看pip命令所在的位置，发现base环境和修改后的那个环境不同即代表修改成功
![pip命令所在的位置](https://od.alonesoul.club/api?path=/Blog/20210925/pip%E5%91%BD%E4%BB%A4%E4%BD%8D%E7%BD%AE.jpg&raw=true)
