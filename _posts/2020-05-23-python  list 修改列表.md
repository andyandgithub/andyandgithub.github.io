---
title: 修改list元素
categories: progress
tags : python
toc: true
---
把列表中的元素直接更改、替换。
例子：表面列表aaa中的元素‘黑色’替换成‘黄色’。
```python
aaa=['黑色','红色','白色','黑色']
```
第一种方法（不建议）：

```python
aaa=['黑色','红色','白色','黑色']
aaa=str(aaa)
bbb=aaa.replace("黑色","黄色")
bbb
 
结果：
"['黄色', '红色', '白色', '黄色']"
```

第二种方法：

```python
aaa=['黑色','红色','白色','黑色']
bbb=['黄色' if i =='黑色' else i for i in aaa]
bbb

结果：
['黄色', '红色', '白色', '黄色']
```


第三种方法：（替换批量的元素）

```python
aaa=['黑色','红色','白色','黑色']
ccc=['黑色','红色']
bbb=['黄色' if i in ccc  else i for i in aaa]
bbb

结果：
['黄色', '黄色', '白色', '黄色']
```
第四种方法：（替换多个元素）
```python

aaa=['黑色','红色','白色','黑色']
ccc={'黑色':'黄色','红色':'白色'}
bbb=[ccc[i] if i in ccc else i for i in aaa]
bbb

结果：
['黄色', '白色', '白色', '黄色']
```


