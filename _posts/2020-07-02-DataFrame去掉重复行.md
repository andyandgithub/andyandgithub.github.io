---
title: DataFrame重复数据
categories: progress
tags : python
toc: true
---



`df.duplicated()`判断是否有重复项
`df.drop_duplicates()`有重复项移除重复项



```python
columns=["id","name","age","score","subject","teacher"]
l=[["000001","yummy",45,456,"chinese","t.wang"],["000001","yummy",45,456,"chinese","t.wang"],
   ["000002","tom",12,789,"math","t.he"],
   ["000001","yummy",45,-26,"math","t.he"],
   ["000002","tom",12,34,"chinese","t.wang"]]
a=pd.DataFrame(l,columns=columns)
d=a.duplicated()
b=a.drop_duplicates()
a.duplicated(['id'])   
a.drop_duplicates(['subject'])
#我们也可以对特定的列进行重复项判断。

ad = a.drop_duplicates(subject=['chinese', 'math'], keep='first')
#去掉subject列为chinese和math的并且保留第一个
```
当`keep=False`时，就是去掉所有的重复行 
当`keep='first'`时，就是保留第一次出现的重复行 
当`keep='last'`时就是保留最后一次出现的重复行。