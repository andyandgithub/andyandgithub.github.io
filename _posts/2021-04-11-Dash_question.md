---
layout: post
title: dash开发中的问题
categories: study
tags : python
toc: true

---

### 缓冲标志

```python
html.Div(dcc.Loading(
                        id="loading-1",
                        type="cube", color="#ff884d", loading_state={"component_name": "loading"},
                        children=html.Div(id="loading")
                    )
 
@app.callback(Output('loading', 'children'), Input('start', 'n_clicks'))
def loading(n_clicks):
    if n_clicks:
        time.sleep(10)#缓冲存在时间长短
```

**`type`**（*值等于：“ graph”，“ cube”，“ circle”，“ dot”，“ default”*； default `'default'`）：确定哪个微调器显示不同种类的加载器

### display

```python
style={"width": "70rem","display":"none"}
```

### 回调之间共享数据、

为了在多个python进程或服务器之间安全地共享数据，我们需要将数据存储在每个进程可访问的位置。

有三个主要位置可存储此数据：

1-通过[dcc.Store](https://dash.plotly.com/dash-core-components/store)在用户的浏览器会话中

2-在磁盘上（例如在文件或数据库中）

3-在跨进程和服务器（如Redis数据库）之间共享的服务器端内存中。为此，[Dash Enterprise](https://plotly.com/dash)包括板载一键式Redis数据库。

如果数据很大，则通过网络发送计算的数据可能会很昂贵。在某些情况下，序列化此数据和JSON可能也很昂贵。

在许多情况下，您的应用只会显示计算或过滤后的数据的子集或汇总。在这些情况下，您可以在数据处理回调中预先计算聚合，然后将这些聚合传输到其余的回调中



服务器上基于用户的会话数据 flask_caching



#### dcc.Store属性

**`id`**（*字符串*；必填）：此组件的ID，用于标识回调中的破折号组件。该ID在应用程序中的所有组件之间都必须是唯一的。

**`clear_data`**（*布尔值*；默认`False`）：设置为True以删除中包含的数据`data_key`。

**`data`**（*字典|列表|数字|字符串|布尔值*；可选）：ID的存储数据。

**`modified_timestamp`**（*数字*；默认`-1`）：上次修改存储的时间。

**`storage_type`**（*值等于：“ local”，“ session”，“ memory”*；默认值`'memory'`）：Web存储的类型。memory：仅保存在内存中，在页面刷新时重置。local：window.localStorage，退出浏览器后保留数据。session：window.sessionStorage，退出浏览器后将清除数据。