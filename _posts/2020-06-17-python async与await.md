---
title: python async与await
categories: progress
tags : python
toc: true

---

# python 函数形式
- 1. 普通函数
```
def function():
    return 1

```
- 2. 生成器函数

```
def generator():
    yield 1

```
在3.5过后，我们可以使用async修饰将普通函数和生成器函数包装成异步函数和异步生成器。
- 3. 异步函数（协程）
```
async def async_function():
    return 1
```
- 4. 异步生成器
```
async def async_generator():
    yield 1
```
## 判断函数类型
```
#通过类型判断可以验证函数的类型

import types
print(type(function) is types.FunctionType)
print(type(generator()) is types.GeneratorType)
print(type(async_function()) is types.CoroutineType)
print(type(async_generator()) is types.AsyncGeneratorType)
```
协程需要通过其他方式来驱动，因此可以使用这个协程对象的send方法给协程发送一个值：
`print(async_function().send(None))`
不幸的是，如果通过上面的调用会抛出一个异常：
`StopIteration: 1`
因为生成器/协程在正常返回退出时会抛出一个StopIteration异常，而原来的返回值会存放在StopIteration对象的value属性中，通过以下捕获可以获取协程真正的返回值：
```
try:
    async_function().send(None)
except StopIteration as e:
    print(e.value)
```
在协程函数中，可以通过await语法来挂起自身的协程，并等待另一个协程完成直到返回结果
















完成异步的代码不一定要用async/await，使用了async/await的代码也不一定能做到异步，async/await是协程的语法糖，使协程之间的调用变得更加清晰，
