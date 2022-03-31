---
title: MiniConda换源
date: 2021-09-23 20:09:09
tags:
  - Python
  - Linux
  - Conda
categories:
  - Python
summary: MiniConda换源
---
### Windows
#### 方法一
-  打开`CMD`依次输入如下命令：
```shell
# 添加清华Conda镜像
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
# conda install安装Python包时显示来自于哪个源
conda config --set show_channel_urls yes
# 添加pytorch
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud//pytorch/
# 添加conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
# 添加msys2
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
# 添加bioconda
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
# 添加menpo
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/

```

#### 方法二
1. 打开 `CMD`输入 `conda config<br>`
2. 按 <kbd>`win`</kbd>+<kbd>`R`</kbd>打开运行，输入 `%path%`点击确定会打开一个文件夹，找到 `.condarc`，打开输入以下内容（豆瓣源，清华源二选一）
- 豆瓣源
  ```
  channels:
   - defaults
  show_channel_urls: true
  default_channels:
    - http://mirrors.aliyun.com/anaconda/pkgs/main
    - http://mirrors.aliyun.com/anaconda/pkgs/r
    - http://mirrors.aliyun.com/anaconda/pkgs/msys2
  custom_channels:
    conda-forge: http://mirrors.aliyun.com/anaconda/cloud
    msys2: http://mirrors.aliyun.com/anaconda/cloud
    bioconda: http://mirrors.aliyun.com/anaconda/cloud
    menpo: http://mirrors.aliyun.com/anaconda/cloud
    pytorch: http://mirrors.aliyun.com/anaconda/cloud
    simpleitk: http://mirrors.aliyun.com/anaconda/cloud
  ```
- 清华源
  ```
  channels:
    - defaults
  show_channel_urls: true
  default_channels:
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
  custom_channels:
    conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  ```
3. 在终端输入 `conda clean -i`清除缓存


### Conda配置代理
打开`.condarc`，添加如下代码，其中`xxxx`代表代理的端口号
```
proxy_servers: {https: http://localhost:xxxx}
```
