---
layout:     post   				    # 使用的布局
title:      16.0 机器学习模型部署及在线预测 				# 标题 
date:       2018-08-19 				# 时间
author:     子颢 						# 作者
catalog: true 						# 是否归档
tags:								#标签
    - 机器学习
    - 模型部署
    - 在线预测
    - deploy
    - predict
---

# 算法原理

条件随机场（Conditional Random Field，CRF），是在给定输入的条件下，求输出变量的条件概率分布模型。通常使用最广泛的是线性链条件随机场，即通过输入序列预测输出序列（序列标注），形式仍然是对数线性模型。若令X = {x1,x2,...,xn}为观测序列，Y = {y1,y2,...,yn}为与之相应的标记序列，则条件随机场的目标是构建条件概率模型P(Y|X)。

![deploy & predict](/img/CRF-01.png)











# 模型训练

代码地址 <a href="https://github.com/qianshuang/ml-exp" target="_blank">https://github.com/qianshuang/ml-exp</a>

```
def train():
    print("start training...")
    # 处理训练数据
    # train_feature, train_target = process_file(train_dir, word_to_id, cat_to_id)  # 词频特征
    train_data = process_maxent_file(train_dir, word_to_id, cat_to_id)  # 最大熵词频特征
    # train_feature, train_target = process_tfidf_file(train_dir, word_to_id, cat_to_id)  # TF-IDF特征
    # 模型训练
    # model.fit(train_feature, train_target)
    model.train(train_data)


def test():
    print("start testing...")
    # 处理测试数据
    # test_feature, test_target = process_file(test_dir, word_to_id, cat_to_id)
    test_data = process_maxent_file(test_dir, word_to_id, cat_to_id)  # 最大熵词频特征
    # test_feature, test_target = process_tfidf_file(test_dir, word_to_id, cat_to_id)  # 不能直接这样处理，应该取训练集的IDF值
    # test_predict = model.predict(test_feature)  # 返回预测类别
    # test_predict_proba = model.predict_proba(test_feature)    # 返回属于各个类别的概率
    # test_predict = np.argmax(test_predict_proba, 1)  # 返回概率最大的类别标签

    # MaxEnt测试
    wordid_freq_jsons = []
    test_target = []
    for i in range(len(test_data)):
        wordid_freq, label_id = test_data[i]
        test_target.append(label_id)
        wordid_freq_jsons.append(json.dumps(wordid_freq))

    test_predict = model.prob_classify_many(wordid_freq_jsons)

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
# model = naive_bayes.MultinomialNB()
# logistic regression
# model = linear_model.LogisticRegression()   # ovr
# model = linear_model.LogisticRegression(multi_class="multinomial", solver="lbfgs")  # softmax回归
# SVM
# model = svm.LinearSVC()  # 线性，无概率结果
# model = svm.SVC(probability=True)  # 核函数，训练慢
# MaxEnt
model = nltk.classify.MaxentClassifier


train()
test()
```
运行结果：
```
read_category...
read_vocab...
start training...
  ==> Training (100 iterations)

      Iteration    Log Likelihood    Accuracy
      ---------------------------------------
             1          -2.30259        0.100
·············
·············
```

# 社群

- QQ交流群
	![562929489](/img/qq_ewm.png)
- 微信交流群
	![562929489](/img/wx_ewm.png)
- 微信公众号
	![562929489](/img/wxgzh_ewm.png)