---
title: python数字字符串排序
date: 2019-11-07 17:38:26
categories: 
- Python
tags:
- Python
summary: Python数字字符串排序
cover: https://od.alonesoul.club/api?path=/Blog/hexo/python.png&raw=true
---

### 源码：
```python
import os

def sort_key(s):
    #图片的命名方式为'1.jpg',这步提取名字中的数字并返回
    return int(s[:-4])

def str_sort(alist):
    #以sort_key为参数排序
    alist.sort(key=sort_key)
    return alist

if __name__ == '__main__':
    path = 'C:\\Users\\0000\\Desktop\\pic'
    filelist = os.listdir(path)
    str_sort(filelist)
    print(filelist)
    
```
