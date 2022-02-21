---
title: opencv-python
categories: study
tags : python
toc: true
---
### 打开一个图片

打开一个窗口

```python
import cv2 as cv
img=cv.imread(r"E:\pycharmproject\graduation\static\2021_04_28_17_20_40.png")
cv.namedWindow("Image")

cv.imshow("Image",img)
cv.imwrite("mig.png",img)#保存图片
```

v2.imwrite(file，img，num)保存一个图像。第一个参数是要保存的文件名，第二个参数是要保存的图像。可选的第三个参数，它针对特定的格式：对于JPEG，其表示的是图像的质量，用0 - 100的整数表示，默认95;对于png ,第三个参数表示的是压缩级别。默认为3.

注意:

- cv2.IMWRITE_JPEG_QUALITY类型为 long ,必须转换成 int
- cv2.IMWRITE_PNG_COMPRESSION, 从0到9 压缩级别越高图像越小。

- cv2.imwrite('1.png',img, [int( cv2.IMWRITE_JPEG_QUALITY), 95])
- cv2.imwrite('1.png',img, [int(cv2.IMWRITE_PNG_COMPRESSION), 9])



## 切割图片

### 根据四条线

```python
img=img[y:y+h,x:x+w]
```

y竖着，x横着，从上到下，从左到右



