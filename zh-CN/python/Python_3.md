---
name: Python数据科学2
---

## Python数据分析生态

---

### 4. Python数据机器学习工具包

---

#### 理论1: 机器学习的相关概念

机器学习是从数据中提取知识。它是统计学、人工智能和计算机科学交叉的研究领域，也被称为预测分析或统计学习。近年来，机器学习方法已经应用到日常生活的方方面面。从自动推荐看什么电影、点什么食物、买什么商品，到个性化的在线电台和从照片中识别好友，许多现代化网站和设备的核心都是机器学习算法。

机器学习按照其学习形式可分为监督学习和无监督学习两大类。
- 监督学习：利用一组已知类别的样本调整分类器的参数，使其达到所要求性能的过程，也称为监督训练或有教师学习。监督学习是从标记的训练数据来推断一个功能的机器学习任务。常见的监督学习任务有回归分析和统计分类等。
- 无监督学习：现实生活中常常会有这样的问题：缺乏足够的先验知识，因此难以人工标注类别或进行人工类别标注的成本太高。很自然地，我们希望计算机能代我们完成这些工作，或至少提供一些帮助。根据类别未知(没有被标记)的训练样本解决模式识别中的各种问题，称之为无监督学习。常见的无监督学习任务有聚类分析等。

解决机器学习问题通常需要以下几个重要步骤：

1. 数据导入与数据预处理
2. 探索性数据分析（观察数据）
3. 建立分析模型
4. 根据建模作出预测
5. 交叉验证与模型评估

机器学习模型有很多，并且还在不断发展。比较常见的机器学习模型有以下几类：

**用于分类和回归分析的模型**

1. 线性模型
2. 最近邻模型（KNN）
3. 决策树模型
4. 随机森林模型
5. 朴素贝叶斯
6. 支持向量机（SVM）
7. 神经网络模型

**用于聚类的模型**

1. K-means模型
2. 层次聚类模型
3. 密度聚类模型


#### 理论2：scikit-learn简介

scikit-learn 是一个开源项目，它包含许多目前最先进的机器学习算法，每个算法都有详细的文档，也是最有名的 Python 机器学习库之一。它广泛应用于工业界和学术界，可以与其他大量 Python 科学计算工具一起使用。

scikit-learn基于`NumPy`,`SciPy`和 `matplotlib`构建，提供了大量简单易用的数据挖掘和机器学习工具。

`scikit-learn`针对以下几类问题提供了大量有效的算法和工具：

1. 分类问题
2. 回归问题
3. 聚类问题
4. 降维问题
5. 模型选择问题
6. 数据预处理问题

`scikit-learn`与`NumPy`,`SciPy`和 `matplotlib`等工具结合使用，可以迅速建立机器学习模型，解决机器学习问题。

---

#### 实验1：安装scikit-learn工具

1. 在命令行下使用pip工具安装scikit-learn工具，命令如下：

``` bash
pip install scikit-learn
```

2. 等待pip联网下载并安装完成
3. 检查安装是否成功。进入Python交互式环境, 输入下列Python代码, 如无异常且能够得到版本号则代表安装成功。

``` python
import sklearn
print(sklearn.__version__)
```

#### 实验2：使用KNN算法进行鸢尾花分类

1. 在notebook中执行如下代码，通过scikit-learn对鸢尾花进行分类预测

``` python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier

iris_dataset = load_iris()
X_train, X_test, y_train, y_test = train_test_split(
    iris_dataset['data'], iris_dataset['target'], random_state=0)
knn = KNeighborsClassifier(n_neighbors=1)
knn.fit(X_train, y_train)
y_pred = knn.predict(X_test)
print("Test set predictions:\n", y_pred)
print("Test set score: {:.2f}".format(knn.score(X_test, y_test)))
```
