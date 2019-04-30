---
name: Python数据科学1
---

## Python数据分析生态

---

### 3. Python数据科学工具包

---

#### 理论1: Python数据科学生态

Python库中包含了大量高质量的数据科学工具，构成了一个繁荣的数据科学生态系统，也使Python语言逐渐成为科学计算领域的首选语言。

Python的科学计算建立在下列软件包的基础上：

- `Python`，一种通用编程语言。它是一种解释型的动态语言，非常适合交互式程序和快速原型设计，同时功能强大，足以编写大型应用程序。
- `NumPy`，数值计算的基础包。它定义了数组和矩阵类型以及它们的基本操作。
- `SciPy`，数值算法和特定领域的工具箱，包括信号处理，优化，统计和更多的工具集。
- `Matplotlib`，一个成熟且受欢迎的绘图软件包，可提供出版品质的2D绘图以及基本的3D绘图。

在此基础上，Python数据科学生态系统提供了大量数据管理和计算工具，生产力工具以及高性能计算工具。

**数据管理和计算工具**

- `Pandas`: 提供高性能，易于使用的数据结构。
- `SymPy`: 用于符号数学和计算机代数。
- `scikit-image`: 用于图像处理算法的集合。
- `scikit-learn`: 用于机器学习算法和工具的集合。

**生产力工具**

- `IPython`: 一个丰富的交互式界面，可让您快速处理数据和测试想法。
- `Jupyter Notebook`: 提供了基于Web浏览器的交互式笔记本，可在浏览器端编辑和运行Python脚本、显示数据和图表、查看数据分析结果。

**高性能计算工具**

- `Cython`: 扩展了Python语法，以便您可以方便地构建C扩展，既可以加速关键代码，也可以与`C/C++`库集成。
- `Dask`，`Joblib`或`IPyParallel`: 聚焦于数值数据的分布式处理。

下面选取数据科学中最常用的工具进行介绍。

#### 理论2：Jupyter Notebook简介

`Jupyter Notebook`是可以在浏览器中运行代码的交互环境。它提供了适用于捕获整个计算过程的基于Web的应用程序：开发，记录和执行代码，以及输出结果。`Jupyter Notebook`结合了两个组件：

- `Web应用程序`：基于浏览器的工具，用于文档的交互式创作，它结合了解释性文本，数学计算以及丰富的媒体输出。

- `Notebook文档`：Web应用程序中可见的所有内容的表示，包括计算的输入和输出，说明文本、数学、图像和对象的富媒体表示。

**Web应用程序的主要功能**

- 用于代码的浏览器内编辑，具有自动语法突出显示，缩进和制表符自动完成。
- 从浏览器执行代码的能力。
- 使用富媒体表示显示计算结果，例如HTML，LaTeX，PNG，SVG等。例如，matplotlib库呈现的出版品质图均可以在浏览器中显示。
- 使用Markdown标记语言对富文本进行浏览器内编辑。
- 能够使用LaTeX在markdown单元格中轻松包含数学符号，并由MathJax本机渲染。

**Notebook文档**

- Notebook文档包含交互式会话的输入和输出以及原始的程序代码。通过这种方式，Notebook文档可以将计算全过程包括代码和执行结果进行记录。这些文档是内部JSON文件，并以`.ipynb`扩展名保存。由于JSON是纯文本格式，因此可以对其进行版本控制并与他人共享。
- 可以通过`nbconvert`命令将笔记本导出为一系列静态格式，包括HTML，reStructuredText，LaTeX，PDF和slide。

#### 理论3：Numpy简介

NumPy(Numerical Python) 是 Python 语言的一个扩展程序库，支持大量的维度数组与矩阵运算，此外也针对数组运算提供大量的数学函数库。

NumPy 是一个运行速度非常快的数学库，主要用于数组计算，包含：

- 一个强大的N维数组对象 ndarray
- 广播功能函数
- 整合 `C/C++/Fortran` 代码的工具
- 线性代数、傅里叶变换、随机数生成等功能

NumPy 的核心功能是 ndarray 类，即多维（n 维）数组。数组的所有元素必须是同一类型。NumPy 数组如下所示：

``` python
import numpy as np
x = np.array([0, 1, 2, 3])
y = np.array([[0, 1, 2], [3, 4, 5]])
print("x:\n{}".format(x))
print("y:\n{}".format(y))
```

x用于表示向量，y用于表示矩阵，输出结果如下：
![](/docs/img/numpy1.png)

numpy所创建的数组都是ndarray对象，ndarray具有如下比较重要的属性：

- ndarray.ndim: 表示数组的维度。
- ndarray.shape: 用来表示数组中的每个维度的大小。例如，对于一个n行和m列的矩阵，其shape为(n,m)。
- ndarray.size: 表示数组中元素的个数，其值等于shape中所有整数的乘积。
- ndarray.dtype: 用来描述数组中元素的类型，ndarray中的所有元素都必须是同一种类型，如果在构造数组时，传入的参数不是同一类型的，不同的类型将进行统一转化。除了标准的Python类型外，NumPy额外提供了一些自有的类型，如numpy.int32、numpy.int16以及numpy.float64等。
- ndarray.itemsize: 用于表示数组中每个元素的字节大小。

ndarray支持大量运算和操作，包括矩阵的`+`,`-`,`*`等基本运算，也包括很多通用的数学方法和统计函数。借助于Numpy，Python具备了一些矩阵运算工具（如Matlab）的计算能力。大量Python科学计算工具都是在Numpy的基础上进行构建的。


#### 理论4：matplotlib与数据可视化

matplotlib 是 Python 主要的科学绘图库，其功能为生成可发布的可视化内容，如折线图、直方图、散点图等。将数据及各种分析可视化，可以更好地帮助用户理解数据。

在项目早期阶段，通常会进行探索性数据分析以获取对数据的理解和洞察，尤其对于大型高维的数据集，数据可视化着实有助于使数据关系更清晰易懂。

同时在项目结束时，以清晰、简洁和引人注目的方式展示最终结果也是非常重要的，因为受众往往是非技术性客户，只有这样，他们才更容易去理解。

为了演示matplotlib的强大绘图能力，我们先使用numpy在-5到5之间均匀生成20个数字，然后使用np.sin(x)计算这20个数字的正弦函数值，最后调用`plt.plot()`方法进行绘图。

``` python
from matplotlib import pyplot as plt

# 在-10和10之间生成一个数列，共100个数
x = np.linspace(-10, 10, 100)
# 用正弦函数创建第二个数组
y = np.sin(x)
# plot函数绘制一个数组关于另一个数组的折线图
plt.plot(x, y, marker="x")
```
![](/docs/img/numpy2.png)

使用plt内置的plot()方法只要给定了x轴和y轴的数据就可以轻而易举的对其进行可视化。matplotlib在Python数据可视化中有十分重要的作用。

---

#### 实验1：安装Python数据科学工具包

1. 在命令行下使用pip工具安装Numpy、Scipy、matplotlib、pandas等科学计算工具，命令如下：

``` bash
pip install numpy scipy matplotlib ipython pandas jupyter
```

2. 等待pip联网下载并安装完成
3. 检查安装是否成功。进入Python交互式环境,分别输入下列Python代码, 如无异常且能够得到版本号（版本号不需要完全一致）则代表安装成功。

``` python
import pandas as pd
print(pd.__version__)

import matplotlib
print(matplotlib.__version__)

import numpy as np
print(np.__version__)

import scipy as sp
print(sp.__version__)

import IPython
print(IPython.__version__)

import jupyter
print(jupyter.__version__)
```
![](./img/numpy3.png)

#### 实验2：启动Jupyter Notebook环境

1. 在命令行输入如下命令

``` bash
jupyter notebook
```

2. 命令执行成功后，浏览器会弹出jupyter notebook的web页面，如下图所示。

![](/docs/img/notebook1.png)

3. 该页面显示了用户命令行所在目录下的所有文件，在页面右边可通过`new`按钮新建一个notebook，如下图所示。

![](/docs/img/notebook2.png)

4. 新建成功以后会进入一个notebook页面，该页面是一个交互式的python环境，可在code区域输出Python代码。

![](/docs/img/notebook3.png)

5. 输入下列代码，并通过`shift + enter`执行，查看运行结果。

``` python
import numpy as np
x = np.array([0, 1, 2, 3])
y = np.array([[0, 1, 2], [3, 4, 5]])
print("x:\n{}".format(x))
print("y:\n{}".format(y))
```

6. 该notebook页面最终会作为一个文件保存，文件名默认为`Untitled.ipynb`,用户可以在网页左上位置修改文件名。

![](/docs/img/notebook4.png)

#### 实验3：Numpy基本使用

本实验建议在jupyter notebook环境下完成。

1. 导入numpy

``` python
import numpy as np
```

2. 创建数组和矩阵

``` python
a = np.array([0, 1, 2, 3])
print(a)
b = np.array([[0, 1, 2], [3, 4, 5]])
print(b)
```

3. 在多数情况下，我们并不需要一个一个去输入数组元素，在notebook环境下执行下列代码

``` python
# 生成 0..n-1 个连续数字
a = np.arange(10) 
print(a)

# 指定数字的开始元素，结束元素（不包括），步长
b = np.arange(1, 9, 2) 
print(b)

# 从0到1之间均匀生成6个数字
c = np.linspace(0, 1, 6)   
print(c)

# 产生3*3的矩阵，元素均为1
d = np.ones((3, 3))
print(d)

# 产生2*2的矩阵，元素均为0
e = np.zeros((2, 2))
print(e)

# 产生3阶单位矩阵
f = np.eye(3)
print(f)

# 以主对角线为[1,2,3,4]生成4阶矩阵
g = np.diag(np.array([1, 2, 3, 4]))
print(g)

# 随机产生4个数字，服从[0,1]均匀分布
h = np.random.rand(4)
print(h)

# 随机产生4个数字，服从高斯分布
i = np.random.randn(4)      
print(i)  
```

4. numpy数组支持索引方式访问，在notebook环境下执行下列代码

``` python
a = np.arange(10)
print(a)
# 分别访问第1，3和最后1个元素
print(a[0], a[2], a[-1])

# 以主对角线为[0，1，2]生成3阶矩阵
b = np.diag(np.arange(3))
# 访问矩阵第二行第二列的元素
print(b[1,1])
```

5. numpy数组还支持Python经典的切片操作，在notebook环境下执行下列代码

``` python
a = np.arange(10)
print(a)

# 通过切片获取数组a的一部分
# 切片规则为：a[开始位置:结束位置:步长]
# 取出a[2],a[5],a[8]
print(a[2:9:3])

# 切片的步长可以省略，默认为1
# 切片的开始位置可以省略，默认为0
# 切片的结束位置可以省略，默认为数组长度len(a)
print(a[:4])
print(a[1:3])
print(a[::2])
print(a[3:])
```

6. numpy支持大量的矩阵运算和统计，在notebook环境下执行下列代码

``` python
# 矩阵与常数的加法、乘法、乘方运算
a = np.array([1, 2, 3, 4])
print (a + 1)
print (2 * a)
print (2 ** a)

# 矩阵之间的运算
b = np.ones(4) + 1
# 对应位置相加
print(a - b)
# 对应位置相乘，不是矩阵乘法运算
print(a * b)

# 矩阵乘法
c = np.ones((3, 3))
c.dot(c)

# 统计计算
x = np.array([1, 2, 3, 4])
# 求和
print(x.sum())
# 找出最小值
print(x.min())
# 找出最大值
print(x.max())
# 找出最小值的下标
print(x.argmin())
# 找出最大值的下标
print(x.argmax())

y = np.array([[1, 2, 3], [4, 5, 6]])
# 二维合并为一维
print(y.ravel())
# 矩阵转置
print(y.T)
```

#### 实验4：matplotlib基本使用

1. 在notebook中执行下列代码，使用matplotlib画折线图

``` python
%matplotlib inline
import matplotlib.pyplot as plt
x = np.linspace(0, 3, 20)
y = 3 * x
plt.plot(x, y)
```

结果如下图：

![](/docs/img/matplotlib1.png)

2. 在notebook中执行下列代码，使用matplotlib画散点图

``` python
%matplotlib inline
import matplotlib.pyplot as plt
x = np.linspace(0, 3, 20)
y = 3 * x
plt.plot(x, y, 'o')
```

结果如下图：

![](/docs/img/matplotlib2.png)

3. 在notebook中执行下列代码，使用matplotlib画正余弦曲线

``` python
import numpy as np
import matplotlib.pyplot as plt

X = np.linspace(-np.pi, np.pi, 256, endpoint=True)
C, S = np.cos(X), np.sin(X)

plt.plot(X, C)
plt.plot(X, S)

plt.show()
```

结果如下图：

![](/docs/img/matplotlib3.png)