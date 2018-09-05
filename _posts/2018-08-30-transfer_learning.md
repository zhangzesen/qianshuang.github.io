---
layout:     post   				    # 使用的布局
title:      27.0 迁移学习 				# 标题 
date:       2018-08-30 				# 时间
author:     子颢 						# 作者
catalog: true 						# 是否归档
tags:								#标签
    - 深度学习
    - 迁移学习
---

# Reusing Pretrained Layers

在前面的章节我们曾经提到过迁移学习，假如现在我们已经训练好了一个人脸识别的深度学习模型，现在又来了一个新的业务让我们训练一个动物识别的模型，这时我们可以重用人脸识别模型所抽取的低级特征，即使用其前面几层隐藏层的权重初始化新模型，因为低级的细粒度特征大家都一样，可以共享（而且也因为使用了其他领域的样本而使得低级特征更加多样化），我们只需要学习高级的特征而不用从头开始学习所有层级的特征，它可以使训练更快并且只需少量的样本即可达到很好的效果。这种方式又叫做Finetune。
![TL](/img/TL-01.png)
任务越相似，可重用的网络层数就越多。
```
[...] # build new model with the same definition as before for hidden layers 1-3
# init = tf.global_variables_initializer()
# 通过scope取出隐藏层所有变量
reuse_vars = tf.get_collection(tf.GraphKeys.TRAINABLE_VARIABLES, scope="hidden[123]")
reuse_vars_dict = dict([(var.name, var.name) for var in reuse_vars])

original_saver = tf.Saver(reuse_vars_dict) # saver to restore the selected variables within the original model

with tf.Session() as sess:
	# sess.run(init)
	original_saver.restore("./my_original_model.ckpt") # restore layers 1 to 3
	[...] # train the new model
```

因为低层级特征是可以共享的，所以我们加载迁移过来的模型后，一般希望freeze住迁移模型的前几层weights和bias，而只训练后面层的参数。
```
# 让模型只训练后面层的参数
train_vars = tf.get_collection(tf.GraphKeys.TRAINABLE_VARIABLES, scope="hidden[34]|outputs")
training_op = optimizer.minimize(loss, var_list=train_vars)
```

# zero-shot & one-shot & few-shot

我们知道，deep learning是一种data hungry的技术，需要大量的标注样本才能发挥作用，样本量少很容易发生过拟合（参数过多，很容易记住所有样本）。现实世界中，有很多问题是没有这么多的标注数据的，获取标注数据的成本也非常大，例如在医疗领域、安全领域等。<br>
对于我们首节讲到的迁移学习类型，虽然样本量也可以少，但是好歹还有一些样本可以进行finetune，如果样本量继续少，只剩下几个甚至一个，甚至一个都没有的情况下，是否依然可以训练呢？当然答案是肯定的，这个时候的模型训练就叫做few-shot learning（如果只有一个标注样本，称为one-shot，一个样本都没有，叫做zero-shot）。<br>
对于从未见过的新类，只能借助每类少数几个标注样本，这才更接近人类认识新事物的方式，我们教小孩识别苹果，只需要拿出一个或少数几个苹果样本让他学习，而不是拿一大堆苹果放在他面前。<br>

据我们目前所知，其实我们有几种相对可行的解决方案：
- 我们知道SVM是对样本Unbalance不敏感的一种分类算法，但是SVM是一种线性分类器（即使通过核函数映射到高维空间，仍然是线性的），而且核函数的目的是进行特征变换，以得到更多高维特征，神经网络的激活函数是去线性化，并用很多神经元以及深层网络从根本上解决线性不可分问题。
- k-NN也可以用来解决此问题，对于one-shot，取k=2，直接计算距离做分类，但是k-NN还是过于简单粗暴，而且对样本Unbalance以及噪声点相对敏感。

# multi-task leaning

多任务迁移学习。

# multi-language learning

多语言迁移学习。

# 模型训练

代码地址 <a href="https://github.com/qianshuang/NER" target="_blank">https://github.com/qianshuang/NER</a>
```

```
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