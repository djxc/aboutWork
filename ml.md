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