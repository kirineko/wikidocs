---
name: Python基础
---

## Python数据分析生态

---

近年来，数据科学、人工智能等技术迅猛发展，Python 已经成为许多数据科学应用的通用语言。它既有通用编程语言的强大功能，也有特定领域脚本语言（比如 MATLAB 或 R）的易用性。Python 有用于数据加载、可视化、统计、自然语言处理、图像处理等各种功能的库。这个大型工具箱为数据科学家提供了大量的通用功能和专用功能。

本章介绍Python语言及其数据分析领域常见的工具包，包含下列几个部分：

- Python语言环境搭建
- Python语言基本语法
- Python数据科学工具包：Numpy、Matplotlab、Scipy
- Python机器学习工具包：scikit-learn

### 1. Python语言环境搭建

---

#### 理论1：环境搭建

搭建Python语言环境主要有三种方式。第一种方式是通过Python官网下载对应操作系统的安装包进行安装；第二种方式是下载预先打包的Python发行版进行安装；第三种方式是通过包管理工具如Ubuntu的apt，CentOS的yum，macOS的brew等进行安装。下面详细介绍前两种环境搭建方式。

注意：Python有Python 2和python 3两个大版本，这两个大版本并不互相兼容。Python 2将在2020年后停止官方支持，鉴于此，用户如无特殊需要都应当积极使用Python 3.

**方式1：官网安装**

通过Python官网可以找到各个版本的Python安装包，推荐用户选择与当前操作系统匹配的最新稳定版进行下载安装。具体分为以下几步：

1. 访问Python官网: https://www.python.org/
2. 在官网进入Download页面
3. 在下载页面下载当前最新稳定版
4. 运行Python安装包，按照安装向导进行安装即可，在安装过程中勾选`Add Python 3.7 to PATH`选项，把Python加入到环境变量中
5. 安装完成以后进入`命令提示符`，在命令行中输入`python -V`,如果成功显示出Python版本，则安装成功

通过官网安装可以得到最新最纯正的Python工具，是最为正统的安装方式。但在大数据分析中，经常会用到一些第三方工具包，这些工具包需要用户另行下载安装。

**方式2：使用预打包的Python发行版**

使用Python进行科学计算和数据分析时，经常需要使用第三方科学计算工具包，因此有一些Python发行版已经预先打包了这些工具包。用户可以根据需要直接下载安装这些Python发行版，这样就可以直接使用常见的第三方工具包，节省了下载安装第三方工具的时间。下面介绍几个最常见的Python发行版。

- **Anaconda**: 用于大规模数据处理、预测分析和科学计算的 Python 发行版。Anaconda 已经预先安装好 NumPy、SciPy、matplotlib、pandas、IPython、Jupyter Notebook 和 scikit-learn。它可以在 Mac OS、Windows 和 Linux 上运行，是一种非常方便的解决方案。对于尚未安装 Python科学计算包的人，建议使用Anaconda。
- **Enthought Canopy**: 用于科学计算的另一款 Python 发行版。它已经预先装有 NumPy、SciPy、matplotlib、pandas 和 IPython，但免费版没有预先安装 scikit-learn。
- **Python(x,y)**: 专门为 Windows 打造的 Python 科学计算免费发行版。Python(x,y) 已经预先装有 NumPy、SciPy、matplotlib、pandas、IPython 和 scikit-learn。

上述发行版均可在官网下载对应操作系统的安装包，按照安装向导安装即可，安装完成以后通过命令行输入`python -V`，如果成功打出了Python版本则安装成功。

#### 理论2：包管理工具

Python的流行离不开繁荣的Python生态，Python用户可以很方便地获取并在自己的应用中导入第三方软件包。Python用户也可以很方便地发布自己的软件包供他人使用。这些都推动着Python生态的不断繁荣。而这一切都离不开Python的包管理工具。

pip 是 Python 包管理工具，该工具提供了对Python 包的查找、下载、安装、卸载的功能。目前Python安装后已经自带pip，无需另行安装即可直接使用。

可以通过以下命令来判断pip是否已安装：

``` bash
pip --version
```

使用pip可以查找、下载、安装、卸载Python包。下面给出pip的一些常用命令。其中numpy是第三方Python包名，用户应根据实际需要进行替换。

``` bash
# 查找第三方包numpy
pip search numpy

# 安装第三方包numpy
pip install numpy

# 卸载第三方包numpy
pip uninstall numpy

# 显示包详细信息
pip show -f numpy

# 列出已安装的包
pip list

# 使用帮助
pip help
```

---

#### 实验1：下载和安装Python

1. 访问Python官网: https://www.python.org/

2. 在官网进入Download页面

![](/docs/images/py-setup1-win.png)

3. 在下载页面下载当前最新稳定版, 64位windows可选择`Windows x86-64 executable installer`, 32位windows可选择`Windows x86 executable installer`

![](/docs/images/py-setup1-win2.png)

4. 运行Python安装包，按照安装向导进行安装即可。建议选择自定义安装方式，在安装过程中勾选`Add Python 3.7 to PATH`选项，把Python加入到环境变量中

![](/docs/images/py-install1.png)

![](/docs/images/py-install2.png)

![](/docs/images/py-install3.png)

5. 安装完成以后进入`命令提示符`，在命令行中输入`python -V`,如果成功显示出Python版本，则安装成功

![](/docs/images/py-install4.png)

#### 实验2：通过包管理工具安装numpy

1. 检查pip版本

``` bash
pip --version
```

![](/docs/images/pip-check.png)

2. 通过pip下载numpy(需要连接互联网)

``` bash
pip install numpy
```

3. 检查numpy是否已经安装

如需检查numpy是否已经安装成功，可以通过python命令进入交互模式，在交互模式下输入`import numpy`,如果没有提示异常则说明numpy已经安装成功。 

``` bash
# 进入python交互模式
python

> import numpy
```

4. 通过pip卸载numpy

``` bash
pip uninstall numpy
```

### 2. Python语言基本语法

---

#### 理论1：Python程序的运行方式

Python是一种动态强类型语言，Python代码是由Python解释器解释执行的，这是Python和编译型语言如C语言、Java语言的显著区别。由于Python代码可以由Python解释器边解释边执行，Python程序大致可分为交互式和脚本式两种运行方式。

**1. 交互式运行方式**

在命令行中输入`python`命令，就进入了Python交互模式。如下图所示。

![](/docs/images/py-run1.png)

在交互模式下用户每输入一行Python语句，Python解释器会对该语句进行解释执行，如遇输出语句则打印输出结果。这种模式看上去就像是用户和Python解释器在进行对话一样，因此被称为**交互模式**

用户可以在交互模式下尝试输入下列语句。

``` python
a = 100 + 200
print(a)
print('hello world')
```

![](/docs/images/py-run2.png)

在Python交互式命令行下，可以直接输入代码，然后执行，并立刻得到结果。这对于语言学习和快速验证想法都是非常有帮助的。

**2. 脚本式运行方式**

在Python的交互式命令行写程序虽然可以直接得到结果，但问题在于代码没法保存，下次如果还想运行相同代码就还需要重新输入。所以，开发过程中往往不会使用交互方式，而是把代码保存在文件当中，这样，程序就可以反复运行了。这种运行方式就是**脚本运行方式**。

可以使用任意的代码编辑器或IDE书写Python脚本，常用的Python代码编辑工具有`VIM`, `Sublime text`, `VS Code`， `PyCharm`等工具，可结合自己的使用习惯进行选择。

脚本方式运行Python程序可分为以下几步：

1. 使用代码编辑器编写Python代码
2. 以`.py`为扩展名保存代码文件
3. 在命令行中找到代码所在目录
4. 通过`python <文件名>`命令，执行上述脚本

脚本运行方式是Python程序最常见的运行方式之一。读者可以尝试把上述交互模式下的代码保存为`test.py`文件，然后通过`python test.py`命令执行并查看结果。

#### 理论2：Python语言基本语法

掌握Python语法是书写Python程序的基础。为了更直观地说明Python语法，本部分结合一个实例程序对Python语法进行说明。

**问题**

找出1-100中既能被4也能被6整除的所有数，并输出。

**问题分析**

对于上述问题可以首先编写一个函数用于判断一个数能否既被4整除也被6整除，如果能则函数返回`True`, 否则函数返回`False`; 然后编写一个循环从1到100进行遍历，对于遍历到的每个数字，将其作为参数传入上述函数，如果函数返回`True`，则表明该数字符合条件，将其加入到结果列表中；循环结束后，将结果列表打印输出就得到了问题的答案。

**代码**

根据上述思路编写的Python代码如下：

``` python
# can_divide函数: 判断一个数字能否既被4整除又被6整除
def can_divide(num):
    if num % 4 == 0 and num % 6 == 0:
        return True
    else:
        return False

# result是一个列表容器，用于保存结果
result = []
for i in range(1, 101):
    if can_divide(i):
        result.append(i)

print(result)
```

**代码分析**

1. Python程序的格式框架

Python程序使用缩进来表示语法层级，这是Python语言非常独特的格式框架，缩进是语法的一部分，缩进不正确程序会运行错误。在Python中，缩进是表达代码间包含和层次关系的唯一手段，通过缩进可以清晰地看出代码块之间的所属关系。

``` python
for i in range(1, 101):
    if can_divide(i):
        result.append(i)
```

在上述代码中，整个if语句都是for循环所属的内容，而`result.append(i)`又是if判断所属的内容，代码所属关系通过缩进的方式清晰地表示出来。熟悉Java或者PHP的读者可以把每一级缩进理解为大括号，每一级缩进的内容都可以看成是大括号中的代码块。

使用Python编程需要格外注意缩进，尤其对于初学者而言，很多程序运行错误都是由缩进错误引发的。

2. 数据类型

Python可以直接处理的基本数据类型包括以下几类：

- 整数：既支持十进制，也支持二进制、八进制、十六进制
- 浮点数：支持直接表示和科学计数法表示
- 字符串：可以用单引号表示，也可以用双引号表示
- 布尔值：用True或者False表示
- 空值：用None表示，需要注意空值和0不同

除了基本数据类型，Python还提供了列表、元组、字典、集合等容器数据类型：

- 列表：是一种有序的集合，可以随时添加和删除其中的元素。如`list = [1,2,3]`就定义了一个包含3个元素的列表，可以使用`append`和`pop`方法添加和删除元素。
- 元组：也是一种有序的集合，但元组是不可变的，因而没有`append()`, `insert()`这样的方法。如`tuple = (1,2,3)`就定义了一个包含3个元素的元组。
- 字典：一种键-值（key-value）存储结构，具有极快的查找速度，可以通过key快速查找value。如`dict = {'China' : 13, 'America' : 4, 'Japan' : 1.5}`就定义了包含3组数据的字典。
- 集合：数学意义上的无序和无重复元素的集合，可通过{}或set()定义。如`test = {1,2,3}`就定义了一个包含3个元素的集合。

此外Python还支持自定义数据类型。

3. 变量定义

Python是动态强类型语言，变量无需事先声明类型，且变量类型可以动态变化。如下列代码：

``` python
a = 123
print(a)
a = 'hello world'
print(a)
```

首先定义了一个变量a，并把整数123赋值给a，然后打印输出。接着又把字符串'hello world'赋值给a，然后打印输出。上述代码中，无论是整数还是字符串，变量a都可以保存，变量a的类型不是固定不变的。这充分反映了Python作为动态语言的灵活性。

4. 控制结构

Python支持分支结构和循环结构。

Python中选择结构的基本格式如下，需要注意的是if条件和else条件后面的分号不可省略，否则会出错。

``` python
if <条件判断1>:
    <执行1>
else:
    <执行2>
```

在上述实际问题中，通过选择结构来判断一个数能否既被4整除又被6整除。

``` python
if num % 4 == 0 and num % 6 == 0:
    return True
else:
    return False
```

Python中的循环结构主要有`for..in`结构和`while`结构两种，通常`for..in`结构常与可迭代对象结合使用，`while`结构则和传统意义上的循环比较类似，只要条件满足，就不断循环，条件不满足时退出循环。

在上述实际问题中，通过`range(1, 101)`会返回一个从1开始到100结束的可迭代对象，通过`for..in`结构对该对象进行迭代，`i`就是每次迭代取出的值。对于每次取出的`i`传给函数`can_divide(i)`,根据函数的返回值进行判断，如果函数返回`True`，则把`i`添加到列表容器`result`中。

``` python
for i in range(1, 101):
    if can_divide(i):
        result.append(i)
```

5. 函数

在Python中，定义一个函数要使用def语句，依次写出函数名、括号、括号中的参数和冒号:，然后，在缩进块中编写函数体，函数的返回值用return语句返回。

在上述实际问题中，就定义了一个名为can_divide的函数，该函数接收一个参数，并在执行后返回True或者False.

``` python
def can_divide(num):
    if num % 4 == 0 and num % 6 == 0:
        return True
    else:
        return False
```

作为动态语言，Python函数中的参数和返回值均不限定类型，这就使Python中的函数十分灵活和强大。

6. 输入输出语句

Python中的输入输出主要使用`input()`函数和`print()`函数。

``` python
name = input('请输入你的名字: ')
print('你的名字是:', name)
```

在上述代码中，input函数等待用户输入，并接收用户输入内容，将其赋给变量name保存，然后再通过print将变量name输出。

---

#### 实验1：使用Python语言编写程序，找出1-100中既能被4也能被6整除的所有数，并输出。

1. 新建Python文件`lab1.py`
2. 通过代码编辑器打开`lab1.py`，并编写下列代码

``` python
# can_divide函数: 判断一个数字能否既被4整除又被6整除
def can_divide(num):
    if num % 4 == 0 and num % 6 == 0:
        return True
    else:
        return False

# result是一个列表容器，用于保存结果
result = []
for i in range(1, 101):
    if can_divide(i):
        result.append(i)

print(result)
```

3. 使用命令行在代码目录下执行`python lab1.py`，查看程序的输出结果。

#### 实验2：使用Python语言编写程序，首先计算出`1-13`的整数中`1`出现的次数，在计算出`100-1000`的整数中`1`出现的次数

1. 新建Python文件`lab2.py`
2. 通过代码编辑器打开`lab2.py`，并编写代码。(提示：1~13当中包含1的数字有1、10、11、12、13，因此1一共出现了6次。)
3. 使用命令行在代码目录下执行`python lab2.py`，查看程序的输出结果。

#### 实验3：使用Python语言编写程序，实现一个句子中单词的翻转。

1. 新建Python文件`lab3.py`
2. 通过代码编辑器打开`lab3.py`，并编写代码。(提示：输入句子：'student. a am I', 经过翻转后，输出句子: 'I am a student.')
3. 使用命令行在代码目录下执行`python lab3.py`，查看程序的输出结果。