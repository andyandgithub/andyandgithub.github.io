---
title: sqlalchemy使用
categories: progress
tags : python
toc: true
---

### sqlacodegen
使用sqlacodegen自动生成sqlalchemy的model代码
```bash
sqlacodegen mysql+pymysql://root:123456@192.168.1.14/management >tmp.py"
```

> sqlacodegen需要自行安装，并且每个表需要有主键，否则会出错。




### 增删改查
```python
engine = create_engine('mysql+pymysql://boss:123456@47.93.99.98:3306/personnel')
# 创建DBSession类型:
DBSession = sessionmaker(bind=engine)
```
#### 增加
```python
# 创建session对象:
session = DBSession()
# # 创建新User对象:
new_user = Login(employee_id=100010, employee_name='Tom',pwd="929847")
# 添加到session:
session.add(new_user)
# # 提交即保存到数据库:
session.commit()
# # 关闭session:
session.close()
```
#### 删
```python
session=DBSession()
res=session.query(Login).filter(Login.employee_id==100010).delete()
session.commit()

print(res)
session.close()
```

#### 改
```python

# UPDATE user SET name="NBDragon" WHERE id=20 更新一条数据
# 使用 db_session 执行User表 query(User) 筛选 User.id = 20 的数据 filter(User.id == 20)
# 将name字段的值改为NBDragon update({"name":"NBDragon"})
res = db_session.query(User).filter(User.id == 20).update({"name":"NBDragon"})
print(res) # 1 res就是我们当前这句更新语句所更新的行数
# 注意注意注意
# 这里一定要将db_session中的执行语句进行提交,因为你这是要对数据中的数据进行操作
# 数据库中 增 改 删 都是操作,也就是说执行以上三种操作的时候一定要commit
db_session.commit()
db_session.close()
#关闭会话

# 更新多条
res = db_session.query(User).filter(User.id <= 20).update({"name":"NBDragon"})
print(res) # 6 res就是我们当前这句更新语句所更新的行数
db_session.commit()
db_session.close()



```
#### 查
```python

# 创建Session:
session = DBSession()
# 创建Query查询，filter是where条件，最后调用one()返回唯一行，如果调用all()则返回所有行:
user = session.query(Login).filter(Login.employee_id==10000).one()

# 打印类型和对象的name属性:
print('type:', type(user))
print('name:', user.employee_name)
# 关闭Session:
session.close()
**equals:**  

​```python
query.filter(User.name=='ed')
```

**not equals:**

```python
query.filter(User.name!='ed')
```

**like:**

```python
query.filter(User.name.like('%ed%'))
```

**in:**

```python
query.filter(User.name.in_(['ed','wen','jask']))
```

**not in:**

```python
query.filter(~User.name.in_(['ed','wen','jask']))
```

**in null:**

```python
 query.filter(User.name==None)
```

**is not null:**

```python
query.fillter(User.name!=None)
```

**and:**

```python
query.filter(and_(User.name=='ed',Username=='ed jones'))
```

**or:**

```python
query.filter(or_(User.name= 'ed',User.name=='wendy')
```

**match:**

```python
query.filter(User.name.match('wendy'))
```

match() 使用特定于数据库的匹配或包含函数;它的行为会随着后端而变化，并且在诸如SQLite这样的一些后端是不可用的。

**all():** 

  返回一个列表

**first():**

  返回至多一个结果，而且以单项形式，而不是只有一个元素的tuple形式返回这个结果

**one():**

  返回且仅返回一个查询结果。当结果的数量不足一个或者多于一个时会报错

**`one_or_one():`**

  当结果数量为0时返回`None`， 多于1个时报错

**`scalar():`**

  成功则返回该行的第一列的列号

**text():**

```python
from sqlalchemy import text

for user in session.query(User)
	.\filter(text("id<224")).\order_by(text("id")).all():

print(user.name)
#ed
#wendy
#mary
#fred
```

**params():**传递参数

```python
>>> session.query(User).filter(text("id<:value and name=:name")).\
  params(value=224, name='fred').order_by(User.id).one()

<User(name='fred', fullname='Fred Flinstone', password='blah')>
```

**from_statement():**直接使用完整的SQL语句，但是要注意将表名和列名写正确

```python
>>> session.query(User).from_statement(text("SELECT * FROM users where name=:name")).\ params(name='ed').all()

#[<User(name='ed', fullname='Ed Jones', password='f8s7ccs')>]
```

**count():**返回符合条件的总数

```python
>>> session.query(User).filter(User.name.like('%ed')).count()
```

**func_count():**可以直接指出要测次数的某一项

```python
>>> from sqlalchemy import func

sql>>> session.query(func.count(User.name), User.name).group_by(User.name).all()

#[(1, u'ed'), (1, u'fred'), (1, u'mary'), (1, u'wendy')]
```