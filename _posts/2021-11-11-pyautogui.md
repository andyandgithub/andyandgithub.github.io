---
layout: post
title: pyautogui
categories: study
tags : python
toc: true

---

# PyAutoGUI——图形用户界面自动化

```python
import pyautogui as pg
pg.size()#返回屏幕宽高比
pg.position()#鼠标位置

img=cv2.imread("path")
position=pg.locateAllOnScreen(img)
position=pg.locateOnScreen("path")#定位图片位置
pg.center(position)#定位图片中心位置

pg.moveTo(posi,duration=1)#鼠标移过去
pg.moveRel(x,y,duration=1)#光标的移动

pg.press("up")#上键
pr.keyUp("up")
pg.keyDown("up")#键盘

pg.mouseUp()
pg.mouseDown()#鼠标

pyautogui.typewrite(s[,duration=t])      #向文本框发送字符串
                                         #可选的duration参数在输入单个字符之间添加短暂的时间暂停
                                         #Attention：只能用于输入英文
 
```

```python
pg.click()# 有错误，偶尔没反应
```

