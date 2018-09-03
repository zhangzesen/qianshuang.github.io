---
layout:     post   				    # 使用的布局
title:      25.0 基于CNN、RNN以及CRF的NER 				# 标题 
date:       2018-08-28 				# 时间
author:     子颢 						# 作者
catalog: true 						# 是否归档
tags:								#标签
    - 深度学习
    - NER
    - CNN-CRF
    - RNN-CRF
---

# 算法原理

前面我们说到，机器翻译是delayed seq to seq，又叫Encoder–Decoder。Encoder模块将原始输入sequence压缩成一个“有意义”的向量表示，Decoder模块将该向量解码为目标输出sequence。
![S2S](/img/S2S-01.png)
下面以一个实际例子来给大家详细介绍Encoder–Decoder模型的基本原理。该示例的目的是输入一个字符序列，输出这个字符序列的字典正序列，比如：输入“bsaqq”，输出“abqqs”。




# 模型训练

代码地址 <a href="https://github.com/qianshuang/dl-exp" target="_blank">https://github.com/qianshuang/dl-exp</a>
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