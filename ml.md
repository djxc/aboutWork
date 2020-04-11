# 机器学习
<p align="center">
  <img src="assets/ico06.png" align="right" width="30">
</p>

## 一、线性回归模型
&emsp;&emsp;1、初始W(权重)，b(偏移项)以及lr(学习率)。  
&emsp;&emsp;2、计算误差，即真实值与预测值之间的差。误差的计算主要有均方差、交叉熵等，其中均方差主要用于回归问题，交叉熵主要用于分类问题。  
&emsp;&emsp;3、声明梯度下降算法：遍历所有的样本，根据权重以及偏移项的导函数计算权重以及偏移项的更新值，然后以权重、偏移项的更新值与学习率乘积来更新权重以及偏移项。  
&emsp;&emsp;4、在设定的循环次数下持续更新权重以及偏移项。  
> &emsp;&emsp;**线性回归模型主要缺点为**：  
&emsp;&emsp;1、线性问题：线性模型只能表达线性关系，而实际中大多数情况属于非线性关系。  
&emsp;&emsp;2、表达能力差：可以通过增加网络层数来提升

&emsp;&emsp;为了解决线性问题，给线性模型加入一个非线性函数，将其转换为非线性。这里的非线性函数通常为激活函数，激活函数可以是sigmoid、ReLU等。   
```java
o = a(Wx + b)
```

## 二、利用tensorflow2实现mnist手写辨识模型
&emsp;&emsp;1、获取数据，初步处理数据。将原始数据缩放到-1 ~ 1之间，并进行batch分组；标签进行one—hot处理。  
&emsp;&emsp;2、创建model以及优化函数，通过keras创建，添加不同的layers；layers中需要设置数据维度以及激活函数(除输出层外)。  
``` javascript
model = keras.Sequential([ 
        layers.Dense(512, activation='relu'),  
        layers.Dense(256, activation='relu'),  
        layers.Dense(10)])  
optimizer = optimizers.SGD(learning_rate=0.001)
```
&emsp;&emsp;3、训练模型，根据设置的循环训练次数，每次循环训练一个batch。每个数据中通过优化函数更新参数。

## 三、处理过拟合现象
&emsp;&emsp;1、将数据集划分为训练集与测试集，然后将训练集在划分为训练集与验证集，一般可以采用6-2-2进行划分。可以根据验证集性能来调整学习率、训练次数、网络结构以及是否过拟合等。  
&emsp;&emsp;2、网络的结构，主要指网络的层数：网络层数过深会过拟合，太浅会出现欠拟合。  
&emsp;&emsp;3、正则化，通过正则化参数调整网络参数，将某些网络参数影响降低，可以解决过拟合问题。  
&emsp;&emsp;4、dropout，将某些网络的连接断开，不是全连接。通过设定断开概率，判断某条连接是否断开。  
&emsp;&emsp;5、数据增强，即在标签不变的数据下，根据先验知识对数据进行一些变换操作，生成新的数据，扩充训练数据集。  

## 四、动手操作
&emsp;&emsp;1、按照书籍，实现卷积网络、可视化等操作。

## 五、学习ssd
* 1、在github中克隆ssd-pytorch项目，在voc2007数据集上可以进行训练，由于gpu显存太小，只能减少batch size。但是当batch size减少之后，出现loss nan，或是回荡。原来学习率与batch size有关系，减少了batch size，响应的学习率也需要修改(较少与batch size倍数一致)。此外学习率的衰减也需要修改，如果衰减很大，那么随着循环次数的增加学习率就很小了，更新不变。
* 2、在训练过程中，可以保存在具体echop的模型参数 torch.save(ssd_net.state_dict(),"xxx.pth")。下次可以在上次保存的模型基础上训练。