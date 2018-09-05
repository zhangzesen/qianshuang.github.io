---
layout:     post   				    # 使用的布局
title:      26.0 基于CNN、RNN以及CRF的NER 				# 标题 
date:       2018-08-29 				# 时间
author:     子颢 						# 作者
catalog: true 						# 是否归档
tags:								#标签
    - 深度学习
    - NER
    - CNN-CRF
    - RNN-CRF
---

# 算法原理

在之前的课程中我们使用原生的CRF模型做完了NER任务，第一步就是特征工程，需要人工构建特征函数，最终效果也跟所选取的特征函数直接相关。
我们知道神经网络的一个重要作用就是特征学习，那么我们能不能用CNN或RNN来提取特征，然后将特征输入给CRF做最终的NER任务呢？答案是肯定的。

我们首先来看下直接用RNN如何做NER任务。
![NNCRF](/img/NNCRF-01.png)
![NNCRF](/img/NNCRF-02.png)
![NNCRF](/img/NNCRF-03.png)
可以看到我们直接对每个cell的输出先做全连接Wrapper（无激活函数，只需拿到score），得到属于每个tag的score，然后计算所有可能的tag序列的score（直接每个tag的score加和），再通过softmax得到每个tag序列的概率，最后通过极大似然估计+随机梯度下降训练模型参数。

但是我们知道一个特定tag和其周围的tag是有关系的，上面的实现方式并没有利用到其周围tag的信息（虽然用bi-RNN可以利用特定输入的过去和未来的上下文信息，但是那是针对输入X的上下文，并不是tag的上下文）。CRF刚好可以cover住这个问题，那么很自然的想到应该两者结合，所以RNN-CRF模型应运而生。

## bi-LSTM-CRF

我们假设状态转移概率矩阵为A（A先要进行初始化，A中的元素即为状态转移概率，随着模型一起训练出来），那么这时每个tag序列的score，就等于bi-LSTM的输出score加上状态转移矩阵作用在该序列上的score，然后通过softmax计算概率。<br>
![NNCRF](/img/NNCRF-06.png)
![NNCRF](/img/NNCRF-07.png)
为了特征多样行，我们还可以加入通过特征工程构造的特征一起来得到输出score：
![NNCRF](/img/NNCRF-04.png)
每个cell的输出做全连接Wrapper时，全连接的输入为concat(bi-LSTM states, 人工feature)。

训练完成后，我们除了得到模型外，还得到了副产品转移概率矩阵，在预测时，我们需要用维特比解码得到最优标记序列。
![NNCRF](/img/NNCRF-05.png)
```
```

## CNN-CRF

如果我们将bi-LSTM-CRF模型中的bi-LSTM替换为CNN，就是我们的CNN-CRF模型。
```
```

# 模型训练

代码地址 <a href="https://github.com/qianshuang/NER" target="_blank">https://github.com/qianshuang/NER</a>
运行结果：
```

```

# 社群

- QQ交流群
	![562929489](/img/qq_ewm.png)
- 微信交流群
	![562929489](/img/wx_ewm.png)
- 微信公众号
	![562929489](/img/wxgzh_ewm.png)