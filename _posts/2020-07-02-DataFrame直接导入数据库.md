---
title: DataFrame直接导入mysql数据库
categories: progress
tags : python
toc: true
---

```python
engine=create_engine('数据库类型+数据库选择://用户名:密码@主机/库名?charset=utf8')
df.to_sql(name='表名', con=engine, if_exists='append', index=False, index_label=False)
```
- `name`: 输出的表名
- `con`: 与read_sql中相同，数据库链接

- `if_exits`： 三个模式：fail，若表存在，则不输出；replace：若表存在，覆盖原来表里的数据；  append：若表存在，将数据写到原表的后面。默认为fail

- `index`：是否将df的index单独写到一列中
- `index_label`:指定列作为df的index输出，此时index为True
- `chunksize`： 同read_sql
- `dtype`: 指定列的输出到数据库中的数据类型。字典形式储存：{column_name: sql_dtype}。常见的数据类型有sqlalchemy.types.INTEGER(), sqlalchemy.types.NVARCHAR(),sqlalchemy.Datetime()等


```python
import pandas as pd
import pymysql
from sqlalchemy import create_engine
# create_engine('数据库类型+数据库选择://用户名:密码@主机/库名?charset=utf8')
engine = create_engine('mysql+pymysql://****:******@******:3306/**?charset=utf8')
#将数据写入sql
# pd.io.sql.to_sql(table_commom,'erp_source',con = engine ,if_exists = 'append',index="False")
columns=["id","name","age","score","subject","teacher"]
l=[["000001","yummy",45,456,"chinese","t.wang"],["000001","yummy",45,456,"chinese","t.wang"],
   ["000002","tom",12,789,"math","t.he"],
   ["000001","yummy",45,-26,"math","t.he"],
   ["000002","tom",12,34,"chinese","t.wang"]]
a=pd.DataFrame(l,columns=columns)
a.to_sql(name='test1', con=engine, if_exists='append', index=False, index_label=False)

```

df.to_sql(name='table', 
          con=con, 
          if_exists='append', 
          index=False,
          dtype={'col1':sqlalchemy.types.INTEGER(),
                 'col2':sqlalchemy.types.NVARCHAR(length=255),
                 'col_time':sqlalchemy.DateTime(),
                 'col_bool':sqlalchemy.types.Boolean
          })



如果不提供`dtype`,`to_sql`会自动根据df列的`dtype`选择默认的数据类型输出，比如字符型会以`sqlalchemy.types.TEXT`类型输出，相比`NVARCHAR`，`TEXT`类型的数据所占的空间更大，所以一般会指定输出为`NVARCHAR`；而如果df的列的类型为`np.int64`时，将会导致无法识别并转换成`INTEGER`型，需要事先转换成int类型（用map，apply函数可以方便的转换）。