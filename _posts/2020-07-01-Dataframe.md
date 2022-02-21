---
title: DataFrame
categories: progress
tags : python
toc: true
---

```python
import pandas
```

# 创建Dataframe

```python
coef_df = pd.DataFrame(self.per_coef_fund_list, columns=["收益水平", "风险系数", "评价指数"], index=["消费能力", "消费偏好", "风险态度系数", "专业水平"])
```

## 获取行列名

- 通过`df.columns`获取列名，并通过`df.columns.tolist()`或者`list(df.columns)`转换为列表

```python
dataframe = pd.DataFrame(np.random.rand(6,4),columns=list('ABCD'))
# 1. 获取dataframe的列名: dataframe.columns
ColNames = dataframe.columns
print(ColNames,type(ColNames)) # 注意ColNames的类型是object
ColNames_List = dataframe.columns.tolist()
print(ColNames_List,type(ColNames_List))
```

通过df.columns.values获取列名，并通过df.columns.tolist()或者list(df.columns)转换为列表

-  获取dataframe的列名: dataframe.columns.values


```python
ColNames = dataframe.columns.values
print(ColNames,type(ColNames)) # 注意ColNames的类型是numpy.ndarray
ColNames_List = dataframe.columns.values.tolist()
print(ColNames_List,type(ColNames_List))
```
-  通过list(df)获取列名，此时获取的结果即为列表

```python
#获取dataframe的列名: list(dataframe)
ColNames = list(dataframe)
print(ColNames,type(ColNames)) #
```

通过[column for column in df]获取列名，此时获取的结果即为列表

-  获取dataframe的列名: [column for column in df]

```python
ColNames = [column for column in dataframe]
print(ColNames,type(ColNames)) #结果以列表形式存储
```



# 数据属性

```python
df.index#行索引
df.columns# 列名
df.shape# 行列大小
df.info()#每列数据的数据信息
df.dtypes# 每一列的类型
df.size#格数 行*列
len(df) #行数

```

# 常用方法

```python
df.head(5)#返回前五行
df.tail(5)#返回后五行
df.raplace({要修带的值:修改后的值})
df["time"].replace("12:00","13:00")#指定列替换
df.sort_values(by = ["",""])# 一句指定列排序，默认升序
df.sort_values(by = ["a","b"],axis=0,ascending=[False,True])#默认先按a降序，再b升序
df.id.value_counts()# 对id列进行统计
df.describe()#描述性统计的相关信息
max(df)#对列名运算
max(df.a)#对a列运算min(),sum(),mean()

```

# 索引

索引值排序
```python
df.sort_index(na_position="last",inplace=True)
```
```python
df.loc[0]#index为0 的一索引名
df.loc[:5]# 提取到索引名为5的行
df.loc[{"a","b"}]#提取名字为a,b两行
```
```python
df.iloc[2:5]#行索引2，3，4  索引号
df.iloc[[1,2,3]]# 1,2,3行
df.iloc[:10,[0,1]]# 提取前十行0，1两列

```

# 数据修改

## 数据查询/过滤

```python
df[df["height"]>50]
df.loc[df["height"]>50]

```

### 分类
```python
df.groupby("id").size()
df.groupby("id")#返回的是series
df.groupby("id").groups #查看每组情况
df.groupby("id")["age"].groups #查看每组情况
```
# 行列处理

## 增加行列

```python
d["c"]=1#增加一列值为1

df.loc['c']=[1,3,1,]#增加行为k的一行值或者修改值

```

## 删除行列

```python
del df["c"]# 删除c列
df=df.drop("k")# 删除"k"行
```
## 修改行列名

```python
df.columns = [新列名]
```

*该方法必须将所有列名全部修改，否则会报错*

```python
df.rename(columns={oldname1:newname1,oldname2:newname2},inplace=True)
```

###  修改索引

1. `set_index`
原索引号直接被替代，teacher列保留在columns里面


```python
df1=df.set_index(["teacher"],drop=False)
DataFrame.set_index(keys, drop=True, append=False, inplace=False, verify_integrity=False) 

```
`append`添加新索引，`drop`为False，`inplace`为True时，索引将会还原为列
可以多个列组成索引，可以设置单索引和复合索引

2. 修改索引列`reset_index`
```python
DataFrame.reset_index(level=None, drop=False, inplace=False, col_level=0, col_fill=”) 
df1=df.reset_index(index.name)
```
`level`控制了具体要还原的那个等级的索引 
`drop`为False则索引列会被还原为普通列，否则会丢失

3. 直接给索引添加名字，不在columns中reset_index可以还原索引，从新变为默认的整型索引 
```python
a.index.name="po"
```
# 缺失值的处理

```python
pd.isnull(df)# 返回所有数据的剑测结果
pd.isnull(df["o"])# 返回o列数据的剑测结果
df.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
df.fillna(vslue=0)用0来填充Null值
```

`df.dropna()   *#删除所有带缺失数据的行`*

- ` parameters`	详解
- `axis`	default 0指行,1为列
- `how`	{‘any’, ‘all’}, default ‘any’指带缺失值的所有行;'all’指清除全是缺失值的行
- `thresh`	int,保留含有int个非空值的行
- `subset`对特定的列进行缺失值删除处理
- `inplace`	这个很常见,True表示就地更改
.

# DataFrame的导出

- xlsx文件`df.to_excel()`
  - excel_writer : 指定文件导出路径与文件名；
    - sheet_name : 指定Sheet名称；
    - index ：设置是否使用自然数索引，默认使用；
    - columns : 指定要导出的列；
    - encoding : 指定编码格式，一般为“utf-8”
    - na_rep : 设置缺失值的填充；
    - inf_rep : 设置无穷值inf的填充值
    - engine : {‘openpyxl’,’xlsxwriter’}，可选，一般为xlsxwriter

```python
df.to_excel(self, excel_writer, sheet_name=’Sheet1′, na_rep=”, float_format=None, columns=None, header=True, index=True, index_label=None, startrow=0, startcol=0, engine=None, merge_cells=True, encoding=None, inf_rep=’inf’, verbose=True, freeze_panes=None)
df.to_excel("news.xlsx")
```

- csv文件`df.to_csv()`
  - path_or_buf : 路径名和文件名；
  - index : 是否使用自然数索引，一般为“False”不使用；
  - columns : 指定要导出的列；
  - sep : 指定分隔符，默认使用逗号；
  - na_rep : 缺失值处理；
  - encoding : 指定编码格式，一般使用utf-8-sig或gbk

```python
df.to_csv(self, path_or_buf=None, sep=’, ‘, na_rep=”, float_format=None, columns=None, header=True, index=True, index_label=None, mode=’w’, encoding=None, compression=’infer’, quoting=None, quotechar='”‘, line_terminator=None, chunksize=None, date_format=None, doublequote=True, escapechar=None, decimal=’.’)
```

- 数据库`df.to_sql()`
  - `name`: 输出的表名
  - `con`: 与read_sql中相同，数据库链接

  - `if_exits`： 三个模式：fail，若表存在，则不输出；replace：若表存在，覆盖原来表里的数据；  append：若表存在，将数据写到原表的后面。默认为fail

  - `index`：是否将df的index单独写到一列中
  - `index_label`:指定列作为df的index输出，此时index为True
  - `chunksize`： 同read_sql
  - `dtype`: 指定列的输出到数据库中的数据类型。字典形式储存：{column_name: sql_dtype}。常见的数据类型有sqlalchemy.types.INTEGER(), sqlalchemy.types.NVARCHAR(),sqlalchemy.Datetime()等

```python
engine=create_engine('数据库类型+数据库选择://用户名:密码@主机/库名?charset=utf8')
df.to_sql(name='表名', con=engine, if_exists='append', index=False, index_label=False)
```

- json`df.to_json()`

  - split 将索引总结到索引，列名到列名，数据到数据。将三部分都分开了了

    ```python
    df.to_json(orient='split')
    ```

  - records 以`columns：values`的形式输出

    ```python
    df.to_json(orient='records')
    ```

  - index 以`index：{columns：values}...`的形式输出

    ```python
    df.to_json(orient='index')
    ```

  - colums 以`columns:{index:values}`的形式输出

    ```python
    df.to_json(orient='columns')
    ```