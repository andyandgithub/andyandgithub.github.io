---
layout: post
title: python question
categories: study
tags : python
toc: true

---

# cmd


### 通过cmd命令来执行python 文件

```bash
python convert_text_list.py E:\file\dataset\Synthetic Chinese String Dataset\train.txt > train_list.txt
```



注意：该路径中包含了一个空格，那么问题来了：按绝对路径执行该python文件时，

将路径加上双引号，

```bash
python convert_text_list.py "E:\file\dataset\Synthetic Chinese String Dataset\train.txt" > train_list.txt
```

### print 输出gbk无法编码

文件头加上

```python
# coding=utf-8
str.encode("utf-8")
```

### 遍历文件

```python
def walkFile(file):
    for root, dirs, files in os.walk(file):

        # root 表示当前正在访问的文件夹路径
        # dirs 表示该文件夹下的子目录名list
        # files 表示该文件夹下的文件list

        # 遍历文件
        for f in files:
            print(os.path.join(root, f))

        # 遍历所有的文件夹
        for d in dirs:
            print(os.path.join(root, d))
```

### python 引入其他文件

1 同目录下直接引用

```python
import train
from train import test
```

2 不同文件夹里引入

```python
import crnn.train
from crnn import train
```

3 引入时可能会报错

新建 \_\_init\_\_.py,在要被引入的目录下即crnn目录下

4 用sys库里的sys.path.append

```python
import sys
sys.path.append('..')
from dir_1.test_1 import test1

```



### 解决python模块调用时代码中使用相对路径访问的文件，提示文件不存在的问题

#### 问题

码过程中使用相对路径使代码的稳定性更好，即使项目目录发生变更，只要文件相对路径不变，代码依然可以稳定运行。但是在python代码中使用相对路径时会存在以下问题

```python
print(os.getcwd())#查看当前目录
```



#### 解决方法

```python
current_path=`
with open(current_path + "/user.txt" , encoding = 'utf-8') as f :
```

## enumerate

enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。

```python
enumerate(sequence, [start=0])
```

- sequence -- 一个序列、迭代器或其他支持迭代对象。

- start -- 下标起始位置。

### python按照文件名数字部分其值的大小进行排序?

```python
path_list.sort(key=lambda x:int(x.split('img_')[1].split('.txt')[0]))
```

## 生成gif图

```python
image_list=[]
  for root, dirs, files in os.walk(image_list_file):
    for f in files:
      image_list.append((os.path.join(root, f)))
  frames = [imageio.imread(image_name) for image_name in image_list]
  # Save them as frames into a gif
  imageio.mimsave(gif_name, frames, 'GIF', duration=0.1)
## duration jian
```

## 读取文件创建修改信息

```python
import time
import datetime
import os
'''把时间戳转化为时间: 1479264792 to 2016-11-16 10:53:12'''
def TimeStampToTime(timestamp):
  timeStruct = time.localtime(timestamp)
  return time.strftime('%Y-%m-%d %H:%M:%S',timeStruct)



'''获取文件的大小,结果保留两位小数，单位为MB'''
def get_FileSize(filePath):
#   filePath = str(filePath,'utf8')
  fsize = os.path.getsize(filePath)
  fsize = fsize/float(1024*1024)
  return round(fsize,2)


'''获取文件的访问时间'''
def get_FileAccessTime(filePath):
#   filePath = str(filePath,'utf-8')
  t = os.path.getatime(filePath)
  return TimeStampToTime(t)


'''获取文件的创建时间'''
def get_FileCreateTime(filePath):
#   filePath = str(filePath,encoding='utf-8')
  t = os.path.getctime(filePath)
  return TimeStampToTime(t)


'''获取文件的修改时间'''
def get_FileModifyTime(filePath):
#   filePath = str(filePath,encoding='utf-8')
  t = os.path.getmtime(filePath)
  return TimeStampToTime(t)
```

##  函数说明文档

```python
def test():
    "我只是测试一下"
    print("test")
 
```

```python
print(help(test))

'''
我只是信测试一下
Help on function hh in module __main__:
-- More  --
'''
```

