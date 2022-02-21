---
title: python csv模块
categories:  progress
tags : python
toc: true
---
## *csv模块*

```python
import csv
```

```python
reader(csvfile, dialect='excel', **fmtparams)
writer(csvfile, dialect='excel', **fmtparams)
参数说明： 
csvfile，必须是支持迭代(Iterator)的对象，可以是文件(file)对象或者列表(list)对象，如果是文件对
象，打开时需要加"b"标志参数。 
dialect，编码风格，默认为excel的风格，也就是用逗号（,）分隔，dialect方式也支持自定义，通过调用register_dialect方法来注册，下文会提到。 
fmtparam，格式化参数，用来覆盖之前dialect对象指定的编码风格。
```
```python
import csv
with open(r'E:\ChromeDownload\winequality-red.csv','r') as myFile:
    lines=csv.reader(myFile)
with open('winequality-red.csv','w')as myfile:
    for line in lines:
         print(line)

with open('t.csv','wb'，newline='') as myFile:      
    myWriter=csv.writer(myFile)  
    myWriter.writerow([7,'g'])  
    myWriter.writerow([8,'h'])  
    myList=[[1,2,3],[4,5,6]]  
    myWriter.writerows(myList)
```
newline防止出现空行。
writerow()方法是一行一行写入，writerows方法是一次写入多行。



```python
register_dialect(name, [dialect, ]**fmtparams)
这个函数是用来自定义dialect的。
参数说明：
name,你所自定义的dialect的名字，比如默认的是'excel'，你可以定义成'mydialect'
[dialect, ]**fmtparams，dialect格式参数，有delimiter（分隔符，默认的就是逗号）、quotechar、
quoting等等，可以参考Dialects and Formatting Parameters
```

```python
csv.register_dialect('mydialect',delimiter='|', quoting=csv.QUOTE_ALL)
```




```python
fr=csv.DictReader(ff)

records=[]
header=list(record.keys())
records.append(record)
fw= csv.DictWriter(ff, fieldnames=header) 
fw.writeheader()
fw.writerows(records)
```
## *pandas对csv的处理*
```python
import pandas as pd		
import numpy as np
```

```python
df = pd.read_csv(filename,usecols=[要取的列名])

# 对于一些数据量比较大的表格,我们可以选择用分块读取的方式来加快读取的速度以及减小系统的内存占用,这种读取方法借鉴于

reader = pd.read_csv(filename, encoding="utf_8_sig", sep=',', iterator=True, usecols=line)
i = 0
while True:
     try:
         start2 = time.clock()
         df = reader.get_chunk(1000000)
         i = i + 1
         end2 = time.clock()
         # 每次循环结束时间
         print('{} 秒: completed {} rows'.format(end2 - start2, i * 1000000))
     except StopIteration:
         print("Iteration is stopped.")
     # 循环结束退出
     break
```

```python
df["User"] = df["ClientID"].map(str)+df["UserID"].map(str)+df["phone"].map(str)

# 我们知道,只有字符串或者数值之间才能直接相加,所以我们要清楚的知道自己合并的这几列是什么格式的数据,如果当做字符串合并,那么就在每列结尾用map(str),如果要用数值相加,那么要用map(float)或者map(int)
# 这样合并原来的列并不会消失,只是会形成一个新列,默认在整个表的尾部,也就是最后一列,如果需要把原列删除再调用drop即可
```
生成新csv
```python
# 当我们读入一些csv表,然后对他们操作完毕之后,我们就需要把处理之后的数据输出,重新生成一个表,就要用到这个函数
df.to_csv(filename, encoding="utf_8_sig")
```