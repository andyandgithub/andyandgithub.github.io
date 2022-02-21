---
title: video Capture python对视频处理
categories: study
tags : python
toc: true

---
```python
import cv2
cap = cv2.VideoCapture(0)
while cap.isOpened():
    ret,frame = cap.read()
    cv2.imshow('frame',frame)
    cv2.imwrite('frame',frame)
    c = cv2.waitKey(1)
    if c == 27:
        break
    
cap.release()
cv2.destroyAllWindows()
```

 程序段里，
 1、`cv2.VideoCapture()`函数：
 `cap = cv2.VideoCapture(0)`
 VideoCapture()中参数是0，表示打开笔记本的内置摄像头。
 `cap = cv2.VideoCapture("…/1.avi")`
 `VideoCapture("…/1.avi")`，表示参数是视频文件路径则打开视频。

2、`cap.isOpened()`函数：
 返回true表示成功，false表示不成功

3、`ret,frame = cap.read()`函数：
 cap.read()按帧读取视频，ret,frame是获cap.read()方法的两个返回值。其中ret是布尔值，如果读取帧是正确的则返回True，如果文件读取到结尾，它的返回值就为False。frame就是每一帧的图像，是个三维矩阵。

4、`cv2.waitKey()`函数：
 参数是1，表示延时1ms切换到下一帧图像，参数过大如cv2.waitKey(1000)，会因为延时过久而卡顿感觉到卡顿。
 参数为0，如cv2.waitKey(0)只显示当前帧图像，相当于视频暂停。
 注意：如果这里没有waitKey()函数，则只会显示空白窗口，看不到摄像头视频。个人分析是因为waitKey()让窗口保持了一定时间，否则前一帧还没有显示出来就开始显示下一帧，就会造成窗口一直是空白（如果是服务端程序无需gui窗口观看视频，则代码中无此函数要求）

5、`cap.release()与destroyAllWindows()`函数：
 cap.release()释放视频，调用destroyAllWindows()关闭所有图像窗口。

6、`c = cv2.waitKey(1)`
 c得到的是键盘输入的ASCII码，esc键对应的ASCII码是27，即当按esc键是if条件句成立。

## 跳帧取图



```python
cap.get(cv2.CAP_PROP_FRAME_COUNT)
```

### `VideoCapture().get()`

get的参数及含义

Note: 如果查询的视频属性是VideoCapture类不支持的，将会返回0

0  视频当前播放位置

cv2.CAP_PROP_FRAME_COUNT   返回视频总帧数

cv2.CAP_PROP_FPS     帧率 每秒展示多少张图片  

cv2.CAP_PROP_FRAME_WIDTH    帧的宽度

cv2.CAP_PROP_FRAME_HEIGH    帧的高度



定好framefrequent

每帧i++，且i%framefrequent==0即使用imwirite

## 比较图片相似度

相邻两个文件比较相似度，相似就把第二个加到新列表(删除列表）里，然后进行新列表去重，统一删除。
　　例如：有文件1-10，首先1和2相比较，若相似，则把2加入到新列表里，再接着2和3相比较，若不相似，则继续进行3和4比较...一直比到最后，然后删除新列表里的图片
   只对于连续图片（例一个视频里截下的图片）准确率也较高，其效率高

```python

import os
import cv2
from skimage.measure import compare_ssim
'''相邻两个文件比较相似度，相似就把第二个加到新列表(删除列表）里，然后进行新列表去重，统一删除。
　　例如：有文件1-10，首先1和2相比较，若相似，则把2加入到新列表里，再接着2和3相比较，若不相似，则继续进行3和4比较...一直比到最后，然后删除新列表里的图片
   只对于连续图片（例一个视频里截下的图片）准确率也较高，其效率高'''
path = r'./E/' # 文件所在路径
img_path = path
imgs_n = [] # 新建的删除列表
i = 0 # 用来统计删了第几张图片
img_files = [os.path.join(rootdir, file) for rootdir, _, files in os.walk(path) for file in files if
                 (file.endswith('.jpg'))]
for currIndex, filename in enumerate(img_files):
    if not os.path.exists(img_files[currIndex]):
       print('not exist', img_files[currIndex])
       break
       img = cv2.imread(img_files[currIndex])
       img1 = cv2.imread(img_files[currIndex + 1])
       # https://cloud.tencent.com/developer/section/1414961 开发者手册链接
       # 本程序最核心的代码
       # skimage.measure.compare_ssim（X，Y，...）
       # 计算两幅图像之间的平均结构相似性指数
       ssim = compare_ssim(img, img1, multichannel=True)
       # 相似度大于0.9就加入删除列表
       if ssim > 0.9:
           imgs_n.append(img_files[currIndex + 1]) # 加入新建删除列表
           i = i+1 # 每加入一个就会删除
           print(img_files[currIndex], img_files[currIndex + 1], ssim)
           print('删除次数', i)
       # 否则不删除，给出两图片的相似度
       else:
           print('small_ssim',img_files[currIndex], img_files[currIndex + 1], ssim)
       currIndex += 1
       if currIndex >= len(img_files)-1:
           break
   for image in imgs_n:
        os.remove(image)
```

