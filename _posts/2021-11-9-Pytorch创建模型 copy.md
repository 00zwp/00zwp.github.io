---
layout: post
title: Pytorch创建模型
description: ""
subtitle: ""
data: 2021-11-8
author: Fat-Pman 
header-img: img/post-bg/5.jpg
catalog: true
tags: Pytorch, attack and defense,
---

### Pytorch创建模型

<font color=blue>写这篇博客的初衷是因为非常多情况下需要用到pytorch的包，但是每一次调用都需要额外编写函数，评估呀什么的，特别要牵扯上攻击和防御，所以就想写个博客，总结一下，彻底研究这个内容</font>

#### torch模型的定义

一般来说，都会创建一个类（继承torch.nn.Module）作为模型。一开始入门，只需要关注两个函数。
```python
def __init__(self):
#用来完成模型的细节定义
def forward(self,x):
#用来表示模型具体的前向传播过程
```
![model](./img/20211109/1.png)

#### 训练过程

<font color='red'>数据处理会专门另作一篇博客，更新后会放上链接</font>

最简单的训练过程可以利用torch的自带函数，设置优化器，损失函数，如下：

```python 
losses = torch.nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(params=torchmodel.parameters(), lr=1e-3, betas=(0.9, 0.999), eps=1e-8,weight_decay=0, amsgrad=False)
```

之后在完成数据导入后，最关键的就是输入数据（前向传播），计算损失（求梯度），更新权重（反向传播）

```python 
outputs = model(X_train)
optimizer.zero_grad()
loss = losses(outputs, y_train)
loss.backward()
optimizer.step()
```

完整代码可以参见[工程](https://github.com/00zwp/Project211108)

#### 模型保存与读取

保存与读取的时候格式要相互匹配。

```python

```


#### 体会

1. model.eval() 与 model.train()  在模型训练和测试的时候要保持好逻辑关系

2. 如果想要查看模型特定的参数 1 for name in model.state_dict(): 2 model.named_parameters() 3 for layer in model.modules():
    for layers in torchmodel.modules():
    print(layers.requires_grad_()) 固定某层
    一个是设置不要更新参数的网络层为false，另一个就是在定义优化器时只传入要更新的参数。当然最优的做法是，优化器中只传入requires_grad=True的参数，这样占用的内存会更小一点，效率也会更高。

