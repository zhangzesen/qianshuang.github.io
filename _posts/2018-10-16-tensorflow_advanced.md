---
layout:     post   				    # 使用的布局
title:      40.0 tensorflow高级特性				# 标题 
date:       2018-10-16 				# 时间
author:     子颢 						# 作者
catalog: true 						# 是否归档
tags:								# 标签
    - TensorFlow
    - keras
---

# Colaboratory

Colaboratory是google发布的一个托管的Jupyter notebook环境，可以免费使用，它具有以下特点：
1. 完全云端运行。相当于Google在云端帮你申请了一台免费虚拟机，TensorFlow已经预先安装并针对所使用的硬件进行了优化。
2. 在代码单元中设置库和数据依赖项，使用!pip install或!apt-get创建cell。通过这种方式可以让其他人轻松地重现您的设置。
3. 可以与Github一起使用。如果你在Github上有一个很好的ipynb，只需将您的Github路径添加到colab.research.google.com/github/即可，例如colab.research.google.com/github/tensorflow/tensor2tensor/blob/master/tensor2tensor/notebooks/hello_t2t.ipynb将加载位于你自己的Github上的此ipynb，创建一键式链接；你还可以通过File > Save a copy to Github 轻松地将Colab笔记本的副本保存到Github。
4. 共享和协同编辑。Colab笔记本存储在Google云端硬盘中，可以通过协作方式进行共享，编辑和评论，只需单击笔记本右上角的“共享”按钮。
5. GPU硬件加速。默认情况下，Colab笔记本在云端CPU上运行，可以通过Runtime > Change runtime type，然后选择GPU从而使Colab笔记本在云端GPU上运行。
6. 你也可以参考https://research.google.com/colaboratory/local-runtimes.html说明让Colab笔记本使用你的本地机器硬件，这时Colab有权限直接读写本地文件。
7. 如果要使用云端资源，需要将本地训练数据上传到云端。
```
from google.colab import files

uploaded = files.upload()

for fn in uploaded.keys():
    print('User uploaded file "{name}" with length {length} bytes'.format(name=fn, length=len(uploaded[fn])))
```
```
from google.colab import files

with open('example.txt', 'w') as f:
    f.write('some content')

files.download('example.txt')
```
注：后面我们的所有案例都将采用Colaboratory。

# keras

keras是一个基于TensorFlow的高级API接口，相当于在TensorFlow的基础上做了一层封装，它独特的模块化和组合式的编程风格使得TensorFlow更加易用、可读性更好、对用户更加友好，并且使TensorFlow的可拓展性更强而不牺牲灵活性和性能。













下面将我们之前的文本分类的案例用keras进行改写，大家可以对比一下keras到底有多简洁。







代码地址 <a href="https://github.com/qianshuang/dl-exp" target="_blank">https://github.com/qianshuang/dl-exp</a>

# 社群

- 微信公众号
	![562929489](/img/wxgzh_ewm.png)