---
title: Conda多环境配置Jupyter Notebook
date: 2021-09-25 13:57:08
tags:
  - Python
  - Conda
categories:
  - Python
summary: 为多个Conda环境的Jupyter配置内核
cover: https://od.alonesoul.club/api?path=/Blog/hexo/conda.png&raw=true
---
## Windows

### 1.激活Conda的base环境

在键盘上同时按下 `Win`+`R`，打开运行，输入 `CMD`按下回车键打开终端，在终端中输入 `conda activate base`来激活 `base`环境
![conda激活](https://od.alonesoul.club/api?path=/Blog/20210925/conda%E6%BF%80%E6%B4%BB.jpg&raw=true)

### 2.安装jupyter

在终端输入 `conda install jupyter`来安装 `jupyter`

### 3.创建内核

在终端输入 `python -m ipykernel install --user`，为当前环境创建一个 `jupyter`的内核，如果提示缺少 `ipykernel`先使用 `conda install ipykernel`来安装下 `ipykernel`
![jupyter创建内核](https://od.alonesoul.club/api?path=/Blog/20210925/jupyter%E5%88%9B%E5%BB%BA%E5%86%85%E6%A0%B8.jpg&raw=true)

### 4.为已存在的conda环境创建jupyter内核

在终端输入 `conda activate your_env_name`切换到你想创建的环境中，`your_env_name`为你的环境名，例如下面
![conda切换环境](https://od.alonesoul.club/api?path=/Blog/20210925/conda%E5%88%87%E6%8D%A2%E7%8E%AF%E5%A2%83.jpg&raw=true)
然后在终端输入 `python -m ipykernel install --user --name your_env_name`即可为 `your_env_name`环境创建 `jupyter`内核
![jupyter创建内核2](https://od.alonesoul.club/api?path=/Blog/20210925/jupyter%E5%88%9B%E5%BB%BA%E5%86%85%E6%A0%B82.jpg&raw=true)

### 5.查看所有已安装jupyter的内核

在终端输入 `jupyter kernelspec list`即可列出所有已安装的 `jupyter`的内核
![查看所有已安装的jupyter内核](https://od.alonesoul.club/api?path=/Blog/20210925/jupyter%E5%B7%B2%E5%AE%89%E8%A3%85%E5%86%85%E6%A0%B8.jpg&raw=true)
