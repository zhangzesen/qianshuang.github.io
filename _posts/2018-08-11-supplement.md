---
layout:     post   				    # 使用的布局
title:      08.0 损失函数、梯度下降、最大似然估计、正则化 				# 标题 
# subtitle:   Hello World, Hello Blog # 副标题
date:       2018-08-11 				# 时间
author:     子颢 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 机器学习
    - 损失函数、梯度下降、正则化
---

# 损失函数

![supplement](/img/supplement-01.png)
![supplement](/img/supplement-02.png)
![supplement](/img/supplement-03.png)
![supplement](/img/supplement-04.png)








# 模型训练

代码地址 <a href="https://github.com/qianshuang/ml-exp" target="_blank">https://github.com/qianshuang/ml-exp</a>

```
def train():
    print("start training...")
    # 处理训练数据
    # train_feature, train_target = process_file(train_dir, word_to_id, cat_to_id)  # 词频特征
    train_feature, train_target = process_tfidf_file(train_dir, word_to_id, cat_to_id)  # TF-IDF特征
    # 模型训练
    model.fit(train_feature, train_target)


def test():
    print("start testing...")
    # 处理测试数据
    test_feature, test_target = process_file(test_dir, word_to_id, cat_to_id)
    # test_predict = model.predict(test_feature)  # 返回预测类别
    test_predict_proba = model.predict_proba(test_feature)    # 返回属于各个类别的概率
    test_predict = np.argmax(test_predict_proba, 1)  # 返回概率最大的类别标签

    # accuracy
    true_false = (test_predict == test_target)
    accuracy = np.count_nonzero(true_false) / float(len(test_target))
    print()
    print("accuracy is %f" % accuracy)

    # precision    recall  f1-score
    print()
    print(metrics.classification_report(test_target, test_predict, target_names=categories))

    # 混淆矩阵
    print("Confusion Matrix...")
    print(metrics.confusion_matrix(test_target, test_predict))


if not os.path.exists(vocab_dir):
    # 构建词典表
    build_vocab(train_dir, vocab_dir)

categories, cat_to_id = read_category()
words, word_to_id = read_vocab(vocab_dir)

# kNN
# model = neighbors.KNeighborsClassifier()
# decision tree
# model = tree.DecisionTreeClassifier()
# random forest
# model = ensemble.RandomForestClassifier(n_estimators=10)  # n_estimators为基决策树的数量，一般越大效果越好直至趋于收敛
# AdaBoost
# model = ensemble.AdaBoostClassifier(learning_rate=1.0)  # learning_rate的作用是收缩基学习器的权重贡献值
# GBDT
# model = ensemble.GradientBoostingClassifier(n_estimators=10)
# xgboost
# model = xgboost.XGBClassifier(n_estimators=10)
# Naive Bayes
model = naive_bayes.MultinomialNB()

train()
test()
```
运行结果：
```
read_category...
read_vocab...
start training...
start testing...

accuracy is 0.915000

             precision    recall  f1-score   support

         体育       0.98      0.94      0.96       116
         科技       0.99      0.99      0.99        94
         财经       0.97      0.96      0.96       115
         家居       0.87      0.80      0.83        89
         时尚       0.98      0.89      0.93        91
         游戏       1.00      0.92      0.96       104
         时政       0.88      0.87      0.88        94
         娱乐       0.89      0.96      0.92        89
         教育       0.82      0.90      0.86       104
         房产       0.80      0.90      0.85       104

avg / total       0.92      0.92      0.92      1000

Confusion Matrix...
[[109   0   0   0   0   0   1   0   5   1]
 [  0  93   0   1   0   0   0   0   0   0]
 [  0   0 110   1   0   0   2   0   0   2]
 [  1   0   1  71   1   0   1   1   2  11]
 [  0   0   0   3  81   0   0   4   3   0]
 [  0   1   0   1   0  96   0   1   5   0]
 [  0   0   1   0   0   0  82   0   1  10]
 [  0   0   0   0   0   0   1  85   3   0]
 [  1   0   0   2   1   0   2   4  94   0]
 [  0   0   1   3   0   0   4   1   1  94]]
```

# 社群

- QQ交流群
	![562929489](/img/qq_ewm.png)
- 微信交流群
	![562929489](/img/wx_ewm.png)
- 微信公众号
	![562929489](/img/wxgzh_ewm.png)