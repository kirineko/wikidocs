---
name: Python综合案例
---

## Python数据分析综合案例

---

### 1. 案例问题

---

根据已有数据预测房屋价格。已有数据如下图所示（只显示前几条数据），通过house_price.csv文件提供：

![](/docs/img/anli1.png)

每一行都代表一条房屋数据，如LSTAT代表房屋所在地区人口百分比, AGE代表房屋年限, DIS代表房屋与市中心的距离, CRIM代表房屋所在地区犯罪率，MEDV代表房价, TAX代表税, RM代表房屋的平均房间数。

该数据集是数据科学领域著名的波士顿房价数据集。现要求通过数据分析建立模型，通过模型描述上述各因素和房价的关系。对于任意给定的一条房屋信息数据，能够通过模型尽量准确地预测其房价。


### 2. 问题分析与工具选择

---

上述问题中已知房屋各项数据和房屋价格，是一个典型的监督学习问题。同时，该房价预测问题又属于监督学习中的回归分析问题，因此可以考虑回归模型的建模方法。

本问题选用的工具包括：

- Python3 : 基础工具平台
- Numpy : 数据分析基础库
- Scipy : 数据分析工具支持
- matplotlib : 数据可视化基础库
- seaborn : 数据可视化高级库
- pandas : 数据表示
- scikit-learn : 机器学习库

在已经安装Python3的情况下，可使用pip安装以上工具, 命令如下:

``` bash
pip install numpy scipy matplotlib ipython pandas jupyter scikit-learn seaborn
```

### 3. 数据导入

---

由于数据通过外部文件提供，因此需要首先需要读取数据，通过pandas库提供的`read_csv()`方法可以很方便地读取数据。

``` python
import pandas as pd
# 读取房屋数据集
df = pd.read_csv("house_data.csv")
# 通过 head 方法查看数据集的前几行数据
df.head()
```

read_csv()方法会把数据读取到Pandas的DataFrame对象中，通过df.head()方法可以查看数据的前5行。

![](/docs/img/anli2.png)


### 4. 数据可视化

---

在建立模型之前，通常会进行探索性数据分析以获取对数据的理解和洞察，尤其对于大型高维的数据集，数据可视化着实有助于使数据关系更清晰易懂。

为了更加直观地理解数据集，考虑进行数据可视化，由于房价数据集的数据是一个典型的多维数据，每一条数据包含14项，而在二维平面是不容易直观展示高维数据的。在这种情况下，我们可以考虑绘制两两特征之间的关系。

在上述14个特征中，有些特征的值全为0，我们可以将其忽略，最终我们选取LSTAT(房屋所在地区人口百分比),AGE(房屋年限), DIS(房屋与市中心的距离), CRIM(房屋所在地区犯罪率)，MEDV(房价), TAX(税), RM(房屋的平均房间数)7个特征进行可视化。

使用`matplotlib`库就可以完成数据的可视化，而`seaborn`库可以看作是对`matplotlib`的封装，通过sns.pairplot()方法即可对上述7个特征进行两两对比，代码如下。

``` python
import matplotlib.pyplot as plt
import seaborn as sns

sns.set(context = 'notebook')

#设置维度：LSTAT(人口百分比), AGE(房屋年限), DIS(与市中心的距离), CRIM(犯罪率)，MEDV(房价), TAX(税), RM(平均房间数)
cols = ['LSTAT','AGE','DIS','CRIM','MEDV','TAX','RM']

sns.pairplot(df[cols],height=2)
plt.show()
```

![](/docs/img/anli3.png)

注意观察图中第5行，展示了MEDV(房价)和其它特征的关系，其中通过第5行第1列可以看出MEDV(房价)和LSTAT(人口百分比)呈负相关，通过第5行第7列可以看出MEDV(房价)和RM(平均房间数)呈正相关。

### 5. 数据建模——单因素建模

---

根据上述结论，MEDV(房价)和LSTAT(人口百分比)呈负相关，和RM(平均房间数)呈正相关，由此可以考虑建立`线性回归`模型。

如果只考虑LSTAT(人口百分比)一个因素对房价的影响，即只针对上图中第5行第1列所展示的关系进行建模，那么问题就变得很简单。

通过scikit-learn中的LinearRegression方法可以很容易地建立线性回归模型。下列代码展示了只针对LSTAT(人口百分比)和MEDV(房价)进行建模的过程。

``` python
# 引入线性回归模块
from sklearn.linear_model import LinearRegression

# 给自变量取值
X = df[['LSTAT']].values
# 给因变量取值
y = df['MEDV'].values
# 初始化模型
sk_model = LinearRegression()
# 训练模型
sk_model.fit(X, y)
# 画出回归图
Regression_plot(X, y, sk_model)
# 设置x轴坐标标签
plt.xlabel('Percentage of the population')
# 设置y轴坐标标签
plt.ylabel('House Price')
plt.show()

# 绘图函数
def Regression_plot(X, y, model):
    plt.scatter(X, y, c="blue")
    plt.plot(X, model.predict(X), color="red")
    return None
```

![](/docs/img/anli4.png)

图中红线即为训练出来的线性回归模型，更加直观地反映出MEDV(房价)和LSTAT(人口百分比)呈负相关。

### 6. 数据建模——多因素建模

---

上述建模方式只考虑了人口百分比单个因素，因而并不全面，实际上完全可以使用`线性回归`进行多因素建模，在上述代码中只需要对X的取值进行修改即可完成多因素建模。

此外，在机器学习过程中，为了评估模型的有效性，往往会采用分离训练集和测试集的方法把样本数据随机划分为训练集和测试集，这样就可以在训练集样本上进行模型训练，而通过测试集就可以评估模型的有效性。这种方法被称为交叉验证。

在scikit-learn中，可以使用`train_test_split`方法进行数据集分离，通过参数`test_size`来设置训练集和测试集的比例。

``` python
from sklearn.model_selection import train_test_split
# 制定维度
cols = ['LSTAT','AGE','DIS','CRIM','TAX','RM']
# 给自变量取值
X = df[cols].values
# 给因变量取值
y = df['MEDV'].values
# 将数据集中75%数据归为为训练集，25%归为测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state = 0)
# 初始化回归模型
sk_model = LinearRegression()
# 训练模型
sk_model.fit(X_train, y_train)
```

以上代码建立了`['LSTAT','AGE','DIS','CRIM','TAX','RM']`6个特征和`MEDV(房价)`的线性回归模型，由于是多因素的线性回归，因此模型很难在二维平面上表示，可以通过`sk_model.coef_`查看各个特征的系数，系数越大表示与`MEDV`的相关性越强。

执行`sk_model.coef_`后的结果是：`[-0.5704543 , -0.03139786, -1.02115195, -0.07792912, -0.01071678, 5.06536897]`，由此可见，该模型中RM(房屋的平均房间数)特征和房价的相关程度最大。

### 7. 模型验证与模型评估

---

为了评估上述模型的有效性，通常会采用交叉验证的方式，使用模型分别在训练集和测试集上对房价进行预测，然后用其预测值和真实值进行对比，这样就可以得到训练误差和测试误差。下面代码计算了各个样本的训练误差和测试误差，并使用`matplotlib`进行了可视化。

``` python
# 计算X在训练集上的预测值
y_train_predict = sk_model.predict(X_train)
# 计算X在测试集上的预测值
y_test_predict = sk_model.predict(X_test)
# 画出在训练集上的预测值与训练误差的散点图
plt.scatter(y_train_predict, y_train_predict - y_train, c = 'red', marker = 'x', label = 'Trainning data')
# 画出在测试集上的预测值与测试误差的散点图
plt.scatter(y_test_predict, y_test_predict - y_test, c = 'black', marker = 'o', label = 'Test data')
# 将X轴的坐标标签设置为预测值
plt.xlabel('Predicted values')
# 将y轴的坐标标签设置为误差
plt.ylabel('Residuals')
# 增加一个图例在左上角
plt.legend(loc = 'upper left')
# 画一条平行于x轴，y值为0的直线
plt.hlines(y=0,xmin=-10,xmax=50,lw=1,color='green')
# 设置取值范围
plt.xlim([-10,50])
plt.show()
```

![](/docs/img/anli5.png)

从上图可以看出，大部分点的误差都在0左右，这说明我们建立的线性回归模型在训练集和测试集上的拟合性都比较好，具有一定的有效性。

对于线性回归模型，我们还可以计算`均方误差`和`R2系数`来进行定量评估，一般而言`均方误差`越小说明预测越准确，`R
2系数`越接近1说明模型越有效。`scikit-learn`同样提供了上述方法。

``` python
# 第一种评估的标准：MSE(均方误差)
# #引入均方误差模块
from sklearn.metrics import mean_squared_error
# 输出均方误差
print('MSE train %.3f, test %.3f'%(mean_squared_error(y_train,y_train_predict),mean_squared_error(y_test,y_test_predict)))
#第二种评估标准：r2_score(r2评分)
# # 引入R2评分模块
from sklearn.metrics import r2_score
# 输出r2评分
print('R^2 train %.3f, test %.3f'%(r2_score(y_train,y_train_predict),r2_score(y_test,y_test_predict)))
```

上述代码的运行结果是：

```
MSE train 25.106, test 36.671
R^2 train 0.706, test 0.551
```

由此可以看出，我们建立的模型在训练集上表现要优于测试集，该线性回归模型还有继续优化的空间。感兴趣的读者可以在此基础上对模型进行进一步完善，尽可能提高模型在测试集上的准确性。
