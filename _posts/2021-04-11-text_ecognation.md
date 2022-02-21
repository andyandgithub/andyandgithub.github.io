---
layout: post
title: 在文字识别的机器学习中的问题
categories: study
tags : machineLearning
toc: true

---

# Craft-pytorch

[原文连接]: https://github.com/clovaai/CRAFT-pytorch

### python模块安装

```bash
pip install -r requirements.txt
```

### 运行训练模型

```bash
python test.py --trained_model=[weightfile] --test_folder=[folder path to test images]

python test.py --trained_model=weights/craft_mlt_25k.pth --test_folder=figures
```

第一个是程序里面test.py的路径，第二个数数据集的路径，第三个是存放图片的路径（注意输入的是文件夹名字不是文件名）

## 运行错误

### 导入.pth文件错误如下

```bash
 load_state_dict
    self.__class__.__name__, "\n\t".join(error_msgs)))
RuntimeError: Error(s) in loading state_dict for CRAFT:
```

**加载使用模型时和训练模型时的环境不一致，**这个模型是用GPU训练的，我本是使用的是CPU，所以会有些问题

将``` load_state_dict(state_dict) ```改成 ``` model.load_state_dict(state_dict, False)```,

改为False仅仅是忽略掉这个异常，True为要求我们定义的Model中的键与我们加载模型里面的键严格一致

### 值错误：要解包的值太多（应该返回三个值，这里肯定是返回超过三个值了）

image.shape属性是一个tuple(高,宽,位深)

```bash
\CRAFT-pytorch\imgproc.py", line 38, in resize_aspect_ratio
    height, width, channel = img.shape
ValueError: too many values to unpack (expected 3)
```

更改

```python
    height, width, channel = img.shape[:3]
```

## cpu only

error:  Attempting to deserialize object on a CUDA device but torch.cuda.is_available() is False. If you are running on a CPU-only machine, please use torch.load with map_location=torch.device('cpu') to map your storages to the CPU.

```python
torch.load(model_file)
```

改为

```python
model = torch.load(model_path, map_location='cpu')

```



error:  Torch not compiled with CUDA enabled

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```

