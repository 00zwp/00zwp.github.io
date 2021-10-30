---
layout: post
title: MNIST classify in Keras 
description: "利用keras工具对MNIST数据集分类"
subtitle: ""
data: 2021-10-29
author: Fat-Pman
header-img: img/post-bg/3.jpg
catalog: true
tags: Papers
---

## 利用keras工具对MNIST数据集分类
### first step : 环境配置
    -keras  ——神经网络搭建
    -numpy  ——数据集的处理

### second step ：数据集的导入

各种数据集的导入都需要用到keras的dataset包，如果使用这个包，他会默认前往亚马逊下载数据集，如果需要使用本地包只能修改源码。

或者可以利用其他方法载入数据集，只要在训练前，完成格式转换就可以。

```python
    from keras.dataset import mnist
    (trainX,trainY),(testX,testY)= mnist.load_data(path='./dataset/mnist.npz')  
    #<class 'numpy.ndarray'>  Y:(60000) 需要注意one_hot
    # 同时 样本 像素为255
    from keras.utils import np_utils
    trainY = np_utils.to_categorical(trainY, 10)

    import tflearn.datasets.mnist as mnist
    X, Y, testX, testY = mnist.load_data(one_hot=True)
```
### third

搭建模型，作者也属于初学keras，下面代码所示属于线性网络结构，所以并不复杂，另外可以再加入卷积池化，还可以自定网络的损失函数，这些可以在后面接触。

```python 
    def model():
        model =Sequential()
        model.add(Flatten())
        model.add(Dense(1024))  #三个全连接 + softmax激活
        model.add(Dense(512))
        model.add(Dense(128))
        model.add(Dense(10))
        model.add(Activation('softmax'))
        model.compile(loss='categorical_crossentropy',optimizer='adadelta',metrics=['accuracy'])
        #利用已有的交叉熵损失函数，以及优化器训练。
        return model
```
<font color=red>**特别需要注意全连接层的使用，要加上平滑操作。**</font>

![Dense](/img/20211029/1.jpg)

### fourth

搭建完模型就可以开始训练并保存模型了。
```python 
    model = model() #调用函数能到神经网络
    model.fit(trainX, trainY, batch_size=256, epochs=12,verbose=1, validation_data=(testX, testY))

    model.save('path/to/location') 
```
这种保存方式会将所有有关模型的信息保存下来。

更多保存方式的介绍可以如图。

！[savemodel](/img/20211029/2.png)

### fifth
训练完成后可以利用保存的模型，加载查看效果。

```python
    model =  keras.models.load_model('./Dense_4/model')
    score = model.evaluate(testX, testY, verbose=0)
    print('Test loss:', score[0])
    print('Test accuracy:', score[1])
```

##  Problems
    需要弄清楚keras.models与自己建立model之间的关联。
    保存与读取模型的各种办法
    如何对单层进行抽取
    如何使用GPU
