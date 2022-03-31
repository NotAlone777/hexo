---
title: Conda常用命令
date: 2021-09-24 21:11:01
tags:
  - Python
  - Conda
categories:
  - Python
summary: conda常用的基础命令
---
| 功能                       | 命令                                                                                                                                                                                                |
| :------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 查看版本                   | `conda --version`                                                                                                                                                                                 |
| 更新conda                  | `conda update conda`                                                                                                                                                                              |
| 查看conda帮助              | `conda --help` `<br>` `conda -h`                                                                                                                                                              |
| **环境管理**         |                                                                                                                                                                                                     |
| 创建指定python版本的环境   | `conda create -n your_env_name python=your_python_version` `<br>` eg:`conda create -n deeplearning python=3.8` `<br>`该命令创建了一个名字为 `deeplearning`python版本为 `3.8`的conda环境 |
| conda环境删除              | `conda remove -n your_env_name --all`                                                                                                                                                             |
| 查看已安装的环境           | `conda env list` `<br>` `conda info -e`                                                                                                                                                       |
| 激活某个环境               | `conda activate your_env_name`                                                                                                                                                                    |
| 退出环境                   | `conda deactivate`                                                                                                                                                                                |
| **包管理**           |                                                                                                                                                                                                     |
| 列举当前激活环境下的所有包 | `conda list`                                                                                                                                                                                      |
| 列举指定环境下的所有包     | `conda list -n your_env_name list`                                                                                                                                                                |
| 为当前激活环境安装某个包   | `conda install package_name` `<br>` `pip install package_name`                                                                                                                                |
| 为当前环境安装指定版本的包 | `conda install package_name=package_version` `<br>` eg:`conda install tensorflow=1.20` `<br>` `pip install package_name=package_version`                                                  |
| 为指定环境安装某个包       | `conda install -n package_name`                                                                                                                                                                   |
| 更新所有包                 | `conda update --all`                                                                                                                                                                              |
| 更新指定的包               | `conda update package_name`                                                                                                                                                                       |
| 搜索指定的包               | `conda search package_name`                                                                                                                                                                       |
| 删除安装包和缓存文件       | `conda clean -y -all`                                                                                                                                                                             |
| 删除没用的包               | `conda clean -p`                                                                                                                                                                                  |
