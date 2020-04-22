# Sklearn-Algorithm
## 线性回归(回归)
**简单线性回归(simple linear regression)** 简单线性回归通常就是包含一个自变量x和一个因变量y，这两个变量可以用一条直线来模拟。如果包含两个以上的自变量就叫做**多元回归(multiple regresseion)** 被用来描述因变量y和自变量x以及偏差error之间关系的方程叫做回归模型<br>
**线性回归的目的是要得到输出向量Y和输入特征X之间的线性关系，求出`线性回归系数`。为了得到线性回归系数θ，我们需要定义一个`损失函数`，一个极小化损失函数的优化方法，以及一个验证算法的方法。损失函数的不同，损失函数的优化方法的不同，验证方法的不同，就形成了不同的线性回归算法。**
### 损失函数
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0.png) <br>
对于这个损失函数，一般有梯度下降法和最小二乘法两种极小化损失函数的优化方法，sklearn中提供的LinearRegression用的是最小二乘法，因为最小二乘法在概率论，统计学等课中学过，所以在这里详细讲解梯度下降法。<br>
### [梯度下降算法](https://www.jianshu.com/p/386645b7e03a)
梯度下降法是一种在学习算法及统计学常用的最优化算法，其思路是对theta取一随机初始值，可以是全零的向量，然后不断迭代改变θ的值使其代价函数J（θ）根据梯度下降的方向减小，直到收敛求出某θ值使得J（θ）最小或者局部最小。其更新规则为：
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/%E6%A2%AF%E5%BA%A6%E4%B8%8B%E9%99%8D.png)<br>
其中alpha为学习率，控制梯度的下降速度，一般取0.01，0.03，0.1，0.3，…, J(θ)对θ的偏导决定了梯度下降的方向，将J(θ)带入更新规则中得到：
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/梯度下降2.png)
- 为什么梯度下降可以逐步减小代价函数
 - 假设函数`f(x)`
 - 泰勒展开：`f(x+△x)=f(x)+f'(x)*△x+o(△x)`
 - 令：`△x=-α*f'(x)`   ,即负梯度方向乘以一个很小的步长`α`
 - 将`△x`代入泰勒展开式中：`f(x+△x)=f(x)-α*[f'(x)]²+o(△x)`
 - 可以看出，`α`是取得很小的正数，`[f'(x)]²`也是正数，所以可以得出：`f(x+△x)<=f(x)`
 - 所以沿着**负梯度**放下，函数在减小，多维情况一样。
 根据本案例中的数据得到梯度下降(代价随迭代次数的变化)的结果为![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/梯度下降结果.png)<br>
 ### [数据归一化(Normalization)](https://www.jianshu.com/p/95a8f035c86c)
 - 目的是使数据都缩放到一个范围内，便于使用梯度下降算法
 - 归一化有很多种方法，其中最常见的是**Min-Max归一化，z-score,平均归一化***
 - 归一化是把数据变成（0，1）或者（1，1）之间的小数。主要是为了数据处理方便提出来的，把数据映射到0～1范围之内处理，更加便捷快速。归一化还可以把有量纲表达式变成无量纲表达式，便于不同单位或量级的指标能够进行比较和加权。归一化是一种简化计算的方式，即将有量纲的表达式，经过变换，化为无量纲的表达式，成为纯量。
### 结果
原始数据和调用库函数形成的数据，本例中的模型为y=ax<sub>1</sub>+bx<sub>2</sub>+c，x<sub>1</sub>表示第一列数字，x<sub>2</sub>表示第二列数字，两者为自变量，y为因变量，表示最后一列数字<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/简单线性回归结果.png)<br>
**多元线性回归是用线性的关系来拟合一个事情的发生规律,找到这个规律的表达公式,将得到的数据带入公式以用来实现预测的目的,我们习惯将这类预测未来的问题称作回归问题.机器学习中按照目的不同可以分为两大类:回归和分类.逻辑回归就可以用来完成分类任务。**
## 逻辑回归(用回归方法做分类问题)
**首先要明确一点，逻辑回归不是用来做回归的，而是用回归的方法来做分类任务。**<br>
### 判定函数sigmod以及阈值的选择
按照多元线性回归的思路,我们可以先对这个任务进行线性回归,学习出这个事情结果的规律,比如根据人的饮食,作息,工作和生存环境等条件预测一个人"有"或者"没有"得恶性肿瘤,可以先通过回归任务来预测人体内肿瘤的大小,取一个平均值作为阈值,假如平均值为y,肿瘤大小超过y为恶性肿瘤,无肿瘤或大小小于y的,为非恶性.这样通过线性回归加设定阈值的办法,就可以完成一个简单的二分类任务.但是使用线性的函数来拟合规律后取阈值的办法是行不通的,行不通的原因在于拟合的函数太直,离群值(也叫异常值)对结果的影响过大。<br>
因此在逻辑回归中选用sigmod函数(又称为Logistic函数)![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/sigmod函数.png)作为判别函数,函数图像为![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/sigmod图像.png)该函数具有很强的鲁棒性(健壮和强壮的意思),并且将函数的输入范围(∞,-∞)映射到了输出的(0,1)之间且具有概率意义.`具有概率意义：将一个样本输入到我们学习到的函数中,输出0.7,意思就是这个样本有70%的概率是正例,1-70%就是30%的概率为负例.`<br>
**我们利用线性回归的办法来拟合然后设置阈值的办法容易受到离群值的影响,sigmod函数可以有效的帮助我们解决这一个问题,所以我们只要在拟合的时候把判定函数换成**![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归判定函数.png)，因为g(z)函数的特性,它输出的结果也不再是预测结果,而是一个值预测为正例的概率,预测为负例的概率就是1-g(z).<br>
在逻辑回归中，因为判定函数输出的是一个事件的概率，我们需要设定一个**阈值**来判断这个概率的时间属于哪一类事件，但是在设定阈值的过程中往往是根据实际情况来判断的，不能只是简单的取0.5，比如，我们做了一个肿瘤的良性恶性判断.选定阈值为0.5就意味着,如果一个患者得恶性肿瘤的概率为0.49,模型依旧认为他没有患恶性肿瘤,结果就是造成了严重的医疗事故.此类情况我们应该将阈值设置的小一些.阈值设置的小,加入0.3,一个人患恶性肿瘤的概率超过0.3我们的算法就会报警,造成的结果就是这个人做一个全面检查,比起医疗事故来讲,显然这个更容易接受.<br>
**综上所述，利用逻辑回归进行分类的主要思想是：根据现有数据对分类边界线建立回归公式，以此进行分类,而求解逻辑回归，就是找到一组系数theta(θ)令判定函数预测正确的概率最大**
### 交叉熵损失函数
根据上述判定函数可得每个单条样本预测正确概率的公式:![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归概率公式.png)，若想让预测出的结果全部正确的概率最大,根据[最大似然估计](https://blog.csdn.net/weixin_39445556/article/details/81416133),就是所有样本预测正确的概率相乘得到的P(总体正确)最大,此时我们令逻辑回归判定函数为![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归判定函数2.png)，则单条样本预测正确概率为![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归概率公式2.png)，为了简化计算，我们对L(θ)这个函数采取两边取log对数的方法，得到<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归概率公式3.png)<br>
l(θ)越大，证明我们得到的系数theta越好，因为在函数最优化时我们习惯求最小值，所以我们在l(θ)中加上符号，得到逻辑回归的损失函数，在这里称之为交叉熵损失函数<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/逻辑回归损失函数.png)<br>
### [求解梯度(对损失函数求偏导)](https://blog.csdn.net/weixin_39445556/article/details/83661219)
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/求解梯度函数.png)<br>
交叉熵损失函数的梯度和最小二乘的梯度形式上完全相同,区别在于,此时的拟合函数![](http://latex.codecogs.com/gif.latex?\\\h_{\theta}(x)=g(z)),而最小二乘的拟合函数是![](http://latex.codecogs.com/gif.latex?\\\h_{\theta}(x)=W^{T}X)
### 结语
使用线性模型进行分类第一个要面对的问题就是如何**降低离群值的影响**,而第二大问题就是,在正负例数据比例相差悬殊时预测效果不好.原因来自于逻辑回归交叉熵损失函数是通过最大似然估计来推导出的.使用最大似然估计来推导损失函数,那无疑,我们得到的结果就是所有样本被预测正确的最大概率.注意重点是我们得到的结果是预测正确率最大的结果<br>比如：100个样本预测正确90个和预测正确91个的两组w,我们会选正确91个的这一组.那么,当我们的业务场景是来预测垃圾邮件,预测黄色图片时,我们数据中99%的都是负例(不是垃圾邮件不是黄色图片),如果有两组w,第一组为所有的负例都预测正确,而正利预测错误,正确率为99%,第二组是正利预测正确了,但是负例只预测出了97个,正确率为98%.此时我们算法会认为第一组w是比较好的.但实际我们业务需要的是第二组,因为**正例检测结果才是业务的根本**. 此时我们需要对数据进行`欠采样/重采样`来让正负例保持一个差不多的平衡,或者使用`树型算法`来做分类.一般树型分类的算法对数据倾斜并不是很敏感,但我们在使用的时候还是要对数据进行欠采样/重采样来观察结果是不是有变好.
## 朴素贝叶斯(分类)
### 贝叶斯推断
条件概率是指事件A在另外一个事件B已经发生条件下的发生概率，记P(A|B)。![](http://latex.codecogs.com/gif.latex?\\\P(A|B)=\frac{P(AB)}{P(B)}=\frac{P(B|A)P(A)}{P(B)})，此外根据全概率公式，条件概率的另一种写法是![](http://latex.codecogs.com/gif.latex?\\\P(A|B)=\frac{P(B|A)P(A)}{P(B|A)P(A)+P(B|{A}')P{A}'}) 贝叶斯公式中，P(A)称为"先验概率"（Prior probability），即在B事件发生之前，对A事件概率的一个判断。P(A|B)称为"后验概率"（Posterior probability），即在B事件发生之后，对A事件概率的重新评估。P(B|A)/P(B)称为"可能性函数"（Likelyhood），这是一个调整因子，使得预估概率更接近真实概率。所以，条件概率可以理解成下面的式子：后验概率＝先验概率 ｘ 调整因子，这就是`贝叶斯推断`的含义。我们先预估一个"先验概率"，然后加入实验结果，看这个实验到底是增强还是削弱了"先验概率"，由此得到更接近事实的"后验概率"。因为在分类中，只需要找出可能性最大的那个选项，而不需要知道具体那个类别的概率是多少，所以为了减少计算量，全概率公式在实际编程中可以不使用。
### [朴素贝叶斯分类](https://blog.csdn.net/guoyunfei20/article/details/78911721?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1)
**朴素贝叶斯的思想基础是这样的：对于给出的待分类项，求解在此项出现的条件下各个类别出现的概率，哪个最大，就认为此待分类项属于哪个类别**<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/朴素贝叶斯分类.png)<br>
可以看到，整个朴素贝叶斯分类分成了三个阶段：
* 准备工作阶段，任务是为朴素贝叶斯分类做必要的准备，主要工作是根据具体情况确定特征属性，并对每个特征属性进行适当划分，然后由人工对一部分待分类项进行分类，形成训练样本集合。这一阶段的输入是所有待分类数据，输出是特征属性和训练样本。这一阶段是整个朴素贝叶斯分类中唯一需要人工完成的阶段，其质量对整个过程将有重要影响，分类器的质量很大程度上由特征属性、特征属性划分及训练样本质量决定。
* 分类器训练阶段，这个阶段的任务就是生成分类器，主要工作是**计算每个类别在训练样本中的出现频率及每个特征属性划分对每个类别的条件概率估计**，并将结果记录。其输入是特征属性和训练样本，输出是分类器。这一阶段是机械性阶段，根据前面讨论的公式可以由程序自动计算完成。
* 应用阶段，这个阶段的任务是使用分类器对待分类项进行分类，其输入是分类器和待分类项，输出是待分类项与类别的映射关系。这一阶段也是机械性阶段，由程序完成。
### 数据类型 [举例](https://blog.csdn.net/qiu_zhi_liao/article/details/90671932)
在进行朴素贝叶斯分类时，要注意数据时离散型还是连续型，当特征属性为离散值时，只要很方便的统计训练样本中各个划分在每个类别中出现的频率即可用来估计P(a|y)。当特征属性为连续值时，通常假定其值服从`高斯分布（也称正态分布）`。因此只要计算出训练样本中各个类别中此特征项划分的各均值和标准差，代入正态分布公式即可得到需要的估计值。另一个需要讨论的问题就是当P(a|y)=0怎么办，当某个类别下某个特征项划分没有出现时，就是产生这种现象，这会令分类器质量大大降低。为了解决这个问题，我们引入`Laplace校准`，它的思想非常简单，就是对每类别下所有划分的计数加1，这样如果训练样本集数量充分大时，并不会对结果产生影响，并且解决了上述频率为0的尴尬局面。
### 总结
朴素贝叶斯分类常用于文本分类，尤其是对于英文等语言来说，分类效果很好。它常用于垃圾文本过滤、情感预测、推荐系统等。<br>
实际应用方式：
- 若任务对预测速度要求较高，则对给定的训练集，可将朴素贝叶斯分类器涉及的所有概率估值事先计算好存储起来，这样在进行预测时只需要 “查表” 即可进行判别；
- 若任务数据更替频繁，则可采用 “懒惰学习” (lazy learning) 方式，先不进行任何训练，待收到预测请求时再根据当前数据集进行概率估值；
- 若数据不断增加，则可在现有估值的基础上，仅对新增样本的属性值所涉及的概率估值进行计数修正即可实现增量学习。<br>
[朴素贝叶斯代码来源](https://github.com/Asia-Lee/Naive_Bayes)
## K-近邻算法(K-Nearest Neighbor,KNN分类，也可用于回归)
#### KNN原理
KNN是通过测量不同特征值之间的距离进行分类。它的思路是：如果一个样本在特征空间中的k个最相似(即特征空间中最邻近)的样本中的大多数属于某一个类别，则该样本也属于这个类别，其中K通常是不大于20的整数。KNN算法中，**所选择的邻居都是已经正确分类的对象。**该方法在定类决策上只依据最邻近的一个或者几个样本的类别来决定待分样本所属的类别。也就是说，KNN就是当预测一个新的点属于哪个类别的时候，根据离他最近的K个点来确定这个新点属于哪个类别。<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/KNN示例.png)
#### KNN实现
由上图可以看出，KNN的实现主要是K值的选取和点距离的计算。
##### K值的选取
一般是通过将样本数据按照一定比例拆分出训练用的数据和验证用的数据，比如7：3拆分出部分训练数据和验证数据（交叉验证），从选取一个较小的K值开始，不断增加K的值，然后计算验证集合的方差，最终找到一个比较合适的K值。![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/KNN中k值的选择.png)<br>
可以看出KNN和KMeans不同的是，KNN随着K值的增大损失函数不会一直减小，而且存在一个“拐点”，这是因为当K值增大时，表示新点属于哪一类有了更多的参照标准，分类效果会变好，但是当k超出一定范围时，分类效果变差，因为如果k值过大，比如100个点里选择了90个作为样本，显然这样的意义不大，相当于重新训练样本。
##### 点距离的计算
在KNN中，通过计算对象间距离来作为各个对象之间的非相似性指标，避免了对象之间的匹配问题，在这里距离一般使用欧氏距离或曼哈顿距离：![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/距离公式.png)，所以KNN最简单的实现方法就是将预测点与所有点的距离进行计算，然后将计算结果升序排序，根据K值选取前K位，看这些数中那种类别的最多，最多的类别就是预测点的类别。
#### KNN算法的特点
**KNN是一种非参的，惰性的算法模型。**
* 非参的意思并不是说这个算法不需要参数，而是意味着这个模型不会对数据做出任何的假设，与之相对的是线性回归（我们总会假设线性回归是一条直线）。也就是说KNN建立的模型结构是根据数据来决定的，这也比较符合现实的情况，毕竟在现实中的情况往往与理论上的假设是不相符的。
* 惰性：同样是分类算法，逻辑回归需要先对数据进行大量训练（tranning），最后才会得到一个算法模型。而KNN算法却不需要，它没有明确的训练数据的过程，或者说这个过程很快。
KNN算法的主要优点是模型训练速度快（惰性），对异常值不敏感，预测效果较好，缺点是因为在预测过程中需要存储全部的数据所以对内存需求较高，预测时间较慢。
#### 方法参数
```
def KNeighborsClassifier(n_neighbors = 5,
                       weights='uniform',
                       algorithm = '',
                       leaf_size = '30',
                       p = 2,
                       metric = 'minkowski',
                       metric_params = None,
                       n_jobs = None
                       )
										
- n_neighbors：这个值就是指 KNN 中的 “K”了。前面说到过，通过调整 K 值，算法会有不同的效果。
- weights（权重）：最普遍的 KNN 算法无论距离如何，权重都一样，但有时候我们想搞点特殊化，比如距离更近的点让它更加重要。这时候就需要 weight 这个参数了，这个参数有三个可选参数的值，决定了如何分配权重。参数选项如下：
        • 'uniform'：不管远近，权重都一样，就是最普通的 KNN 算法的形式。
        • 'distance'：权重和距离成反比，距离预测目标越近具有越高的权重。
        • 自定义函数：自定义一个函数，根据输入的坐标值返回对应的权重，达到自定义权重的目的。
- algorithm：在 sklearn 中，要构建 KNN 模型有三种构建方式，1. 暴力法，就是直接计算距离存储比较的那种放松。2. 使用 kd 树构建 KNN 模型 3. 使用球树构建。 其中暴力法适合数据较小的方式，否则效率会比较低。如果数据量比较大一般会选择用 KD 树构建 KNN 模型，而当 KD 树也比较慢的时候，则可以试试球树来构建 KNN。参数选项如下：
		• 'brute' ：蛮力实现
		• 'kd_tree'：KD 树实现 KNN
		• 'ball_tree'：球树实现 KNN 
		• 'auto'： 默认参数，自动选择合适的方法构建模型
不过当数据较小或比较稀疏时，无论选择哪个最后都会使用 'brute'
		
- leaf_size：如果是选择蛮力实现，那么这个值是可以忽略的，当使用KD树或球树，它就是是停止建子树的叶子节点数量的阈值。默认30，但如果数据量增多这个参数需要增大，否则速度过慢不说，还容易过拟合。
- p：和metric结合使用的，当metric参数是"minkowski"的时候，p=1为曼哈顿距离， p=2为欧式距离。默认为p=2。
- metric：指定距离度量方法，一般都是使用欧式距离。
		• 'euclidean' ：欧式距离
		• 'manhattan'：曼哈顿距离
		• 'chebyshev'：切比雪夫距离
		• 'minkowski'： 闵可夫斯基距离，默认参数
- n_jobs：指定多少个CPU进行运算，默认是-1，也就是全部都算
```
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/KNN_distance.png)
## K-Means算法(聚类)
[参考](https://blog.csdn.net/llh_1178/article/details/81633396?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3)
#### 聚类原理
将某一些数据分为不同的类别，在相同的类别中数据之间的距离应该都很近，也就是说离得越近的数据应该越相似，再进一步说明，**数据之间的相似度与它们之间的欧式距离成反比**。这就是k-means模型的假设。 <br>
有了这个假设，我们对将数据分为不同的类别的算法就更明确了，尽可能将离得近的数据划分为一个类别。不妨假设需要将数据{xi}聚为k类，经过聚类之后每个数据所属的类别为{ti}，而这k个聚类的中心为{μi}。于是定义如下的损失函数：![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/kmeans.png)kmeans模型的目的是找寻最佳的{ti}，使损失函数最小，之后就可以对聚类中心{μi}直接计算了。由此可见，它既是聚类的最终结果，也是需要估算的模型参数。
#### 聚类过程
在k-means的损失函数中存在两个未知的参数：一个是每个数据所属的类别{ti}；一个是每个聚类的中心{μi}。这两个未知的参数是相互依存的：如果知道每个数据的所属类别，那么类别的所有数据的平均值就是这个类别的中心；如果知道每个类别的中心，那么就是计算数据与中心的距离，再根据距离的大小可以推断出数据属于哪一个类别。 
- 聚类属于无监督学习，不知道y的标记分为k类
- K-Means算法分为两个步骤：第一步：**簇分配**，随机选K个点作为中心，计算到这K个点的距离，分为K个簇；第二步：**移动聚类中心：**重新计算每个簇的中心(求平均值)，移动中心，重复以上步骤。
#### 类别数量的确定和聚类中心的初始化
一般情况下，随着分类组数的增加，损失函数会减少，但是分类组数过多会造成过拟合，分类组数过少又会造成欠拟合，因此，在确定分类组数之前，可以进行一个数据经过统计，观察损失函数随着K值的增加如何变化，看损失函数随着k值变化的图像上有无拐点，若有拐点，则表示当聚类个数小于拐点值时，误差平方和会下降的很快；当聚类个数超过拐点时，误差平方和虽然会继续下降，但是下降的速度会缓减，而这个转折点就是最佳的聚类个数了，我们也称其为`“肘部法则”`。但是在现实情况中，多数会随着k值的增加损失函数不断减小，不存在明显的拐点。那么这种情况，K值的选择主要还是根据经验以及利用k-means聚类的目的来决定。<br>
对于聚类数目K值较小（K<10）的情况下，我们可以多次随机选取不同聚类中心，最后比较各自迭代完成后的畸变函数值，畸变函数越小，则说明聚类效果更优。但是在k值较大的情况下，比如上百类甚至上千万类，这时候重新选取不同的聚类中心可能就没有很好的效果了。
#### [K-Means算法的应用](https://github.com/lawlite19/MachineLearning_Python#%E4%BA%94k-means%E8%81%9A%E7%B1%BB%E7%AE%97%E6%B3%95)
* **将图片的像素分为若干类，然后用这个类代替原来的像素值，**利用k均值聚类算法，将图片中所有的颜色值做聚类，然后用聚类中心的颜色代替原来的颜色值，形成新的图片。例如：从一张 `256x256 `像素的RGB图像来解释，观测样本xi为图中的每个像素点，共有 256x256=65536 个，RGB图片每个像素点是由（r，g，b）三个通道的值组成（每个值的范围从 0-255），因此每个样本都是3维向量。kmeans压缩方法的本质是**量化矢量 (Vector Quaintization) ，** 通过 kmeans 聚类得到量化表，将每个像素用量化表中的矢量来表示，然后只要记录每个像素对应的索引值，这样原来使用 24bit 来表示一个像素，现在只需要存储记录索引值所需要的 6bit 就可了，因此实现了压缩图像。**`因为每一个像素点都是一个(r,g,b)三维数组，所以通过kmeans算法可以将颜色相近的像素点用聚类中心表示`** <br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/kmeans压缩图片.png)
```
# 聚类算法
def runKMeans(X,initial_centroids,max_iters,plot_process):
    m,n = X.shape                   # 数据条数和维度
    K = initial_centroids.shape[0]  # 类数
    centroids = initial_centroids   # 记录当前类中心
    previous_centroids = centroids  # 记录上一次类中心
    idx = np.zeros((m,1))           # 每条数据属于哪个类
    
    for i in range(max_iters):      # 迭代次数
        print u'迭代计算次数：%d'%(i+1)
        idx = findClosestCentroids(X, centroids)
        if plot_process:    # 如果绘制图像
            plt = plotProcessKMeans(X,centroids,previous_centroids) # 画聚类中心的移动过程
            previous_centroids = centroids  # 重置
        centroids = computerCentroids(X, idx, K)    # 重新计算类中心
    if plot_process:    # 显示最终的绘制结果
        plt.show()
    return centroids,idx    # 返回聚类中心和数据属于哪个类
```
* [对鸢尾花的数据进行聚类](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/K-Means/K-Means_scikit-learn2.py)
通过调用sklearn提供的鸢尾花数据集和kmeans算法，来演示聚类结果。<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/kmeans鸢尾花数据聚类.png)
## PCA主成分分析(降维)
#### 背景
在许多领域的研究与应用中，通常需要对含有多个变量的数据进行观测、收集之后进行分析寻找规律，多变量大数据集中包含丰富的信息，但在一定程度上也会增加收集难度和分析难度，而且在大多数情况下，许多变量之间可能还存在相关性，从而增加了问题分析的复杂性。如果分别对每个指标进行分析，分析往往是孤立的，不能完全利用数据中的信息，因此盲目减少指标会损失很多有用的信息，从而产生错误的结论。<br>
因此需要找到一种合理的方法，**在减少需要分析的指标同时，尽量减少原指标包含信息的损失**，以达到对所收集数据进行全面分析的目的。由于各变量之间存在一定的相关关系，因此可以考虑将关系紧密的变量变成尽可能少的新变量，使这些新变量是两两不相关的，那么就可以用较少的综合指标分别代表存在于各个变量中的各类信息。主成分分析与因子分析就属于这类降维算法。<br>
* 数据降维有以下优点：（1）使数据集更易使用 （2）降低算法的计算开销 （3）去除噪声 （4）使得结果更容易理解<br>
#### [PCA原理详解](https://www.zhihu.com/question/41120789)
PCA(Principal Component Analysis)一种使用最广泛的数据降维算法。PCA的主要思想是将n维特征映射到k维上，这k维是全新的**正交特征**也被称为主成分，是在原有n维特征的基础上重新构造出来的k维特征。PCA的工作就是从原始的空间中顺序地找一组相互正交的坐标轴，新的坐标轴的选择与数据本身是密切相关的。其中，第一个新坐标轴选择是原始数据中方差最大的方向，第二个新坐标轴选取是与第一个坐标轴正交的平面中使得方差最大的，第三个轴是与第1,2个轴正交的平面中方差最大的。依次类推，可以得到n个这样的坐标轴。通过这种方式获得的新的坐标轴，我们发现，大部分方差都包含在前面k个坐标轴中，后面的坐标轴所含的方差几乎为0。于是，我们可以忽略余下的坐标轴，只保留前面k个含有绝大部分方差的坐标轴。事实上，这相当于只保留包含绝大部分方差的维度特征，而忽略包含方差几乎为0的特征维度，实现对数据特征的降维处理。
##### 什么是降维
如下述数据：
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_01.png)<br>
这种一维数据可以放在以为坐标轴上，但是数据需要经过处理，假设房价样本用X表示，均值用x表示，然后以x均值为坐标轴的原点，以x为原点，即x=0，其余数据均减去x，即10-x,2-x...这个过程称为“中心化”。“中心化”处理的原因是，这些数字后继会参与统计运算，比如求样本方差，中间就包含了X<sub>i</sub>-x，结合求方差的公式，用“中心化”的数据就可以直接算出“房价”的样本方差，“中心化”之后可以看出数据大概可以分为两类<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_02.png)<br>
现在新采集了房屋的面积，可以看出两者完全正相关，有一列其实是多余的，求出房屋样本、面积样本的均值，分别对房屋样本、面积样本进行“中心化”后得到：<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_03.png)<br>
房价(X)和面积(Y)的样本协方差是这样的：<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/协方差公式.png)<br>
可见，经过中心化的数据能过简化计算过程，把二维数据画在二维坐标系中，可以看出他们排列成一条直线：<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_04.png)<br>
如果旋转坐标系，让横坐标和这条直线重合：<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_05.webp)<br>
旋转后的坐标系，横纵坐标不再代表“面积”，“房价”，而是两者的混合（线性组合），这里把他们称作主元1，主元2，坐标值很容易用勾股定理(根据直线的斜率求夹角)计算出来，很显然a,b,c,d,e这些点在“主元2”上的坐标为0，把所有的数据换算到新的坐标系上：<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_06.png)<br>
因为主元2上的值全部为0，完全是多余的，因此我们可以舍弃主元2，只保留主元1，这样有把数据降成了一维，而且没有丢失任何信息。
##### 非理想情况下如何降维
上面是比较极端的情况，就是房价和面积完全正比，所以二维数据会在一条直线上。以下面数据为例<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_07.png)<br>
和上面的处理一样，把房价、面积表现在坐标轴上，与上面不一样是，这些点没有完全在同一条直线上，见[image/PCA_08.png](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_08.png)
从线性代数的角度来考虑降维，二维坐标系总有各自的标准正交基（也就是两两正交、模长为1的基，**e<sub>1</sub>，e<sub>2></sub>** ），在某个坐标系中的任意一点q(x,y)都能够用**e<sub>1</sub>，e<sub>2></sub>** 的线性组合来表示，q(x,y)=x**e<sub>1</sub>**+y**e<sub>2</sub>** ，只是在不同的坐标系中，基前的系数不一样。但是q点到原点的距离是不会变化的，因此，在某个坐标系下分配给x较多，分配给y的必定较少，反之亦然，最极端的情况是在某个坐标系下，全部分配给了x，使得y=0。   对于两个点a(x1,y1)，b(x2,y2)，为了降维应该尽可能多的分配给(x1,x2)，较少的分配给(y1,y2)<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/PCA_09.webp)<br>
##### 主元分析
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/主元分析.png)<br>
随着坐标系的不同，X1,X2的值会不断变化<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/主元分析2.png)<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/主元分析3.png)<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/主元分析4.png)<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/主元分析5.png)<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/主元分析6.png)<br>
[PCA的理解来自于此博客](https://www.zhihu.com/question/41120789)
## BP神经网络(原理)
##### 基本概念
神经网络是一种运算模型，由大量的节点（或称神经元）之间相互联接构成。每个节点代表一种特定的输出函数，称为激励函数（activation function）。每两个节点间的连接都代表一个对于通过该连接信号的加权值，称之为权重，这相当于人工神经网络的记忆。网络的输出则依网络的连接方式，权重值和激励函数的不同而不同。而网络自身通常都是对自然界某种算法或者函数的逼近，也可能是对一种逻辑策略的表达。<br>
###### 神经元
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/神经元.png)<br>
这个神经元是由x1,x2,x3和b=+1作为输入，w1,w2,w3作为他们的权重，输入节点后，通过激活函数F，得到输出
![](http://latex.codecogs.com/gif.latex?\\h_{W,b}(x)=f(W^Tx)=f(\sum\nolimits_{i=1}^{3}W_ix_i+b))
对于一个神经元来说，整个过程就是，向神经元输入数据，然后经过激活函数，对数据做某种转换，最终得到一个输出结果。
###### 神经网络
所谓神经网络就是将许多个单一“神经元”联结在一起，这样，一个“神经元”的输出就可以是另一个“神经元”的输入。例如，[下图](https://github.com/lawlite19/MachineLearning_Python/blob/master/images/NeuralNetwork_01.png)就是一个简单的神经网络： 
![](https://github.com/lawlite19/MachineLearning_Python/blob/master/images/NeuralNetwork_01.png)<br>
* a<sub>i</sub><sup>(j)</sup>表示第j层的第i个激励，也称为为单元unit
* W<sup>(j)</sup>为第j层到第j+1层映射的权重矩阵，就是每条边的权重
最左边一层是输入层，中间是隐藏层，右侧是输出层。输入层以及隐藏层均有3个节点，每个节点代表一个神经元。输入层有3个输入节点，x1, x2, x3， 以及一个偏置节点（标有+ 1的圆圈）。每一层和下一层之间对应的也有一个权重矩阵w。<br>
对于这样一个简单的神经网络来说，我们的整个过程就是，将输入x与权重矩阵w结合，以wx + b的形式输入隐藏层（Layer L2），经过激活函数f(x)的处理，得到输出结果a1, a2, a3， 然后与对应的权重，偏置结合，作为输出层（Layer L3）的输入，经过激活函数，得到最终输出结果。<br>
在神经网络中每一个神经元就是三件事：输入，判断和输出。输入层的神经元（就是那个圆形的圈，代表一个神经元或者一个神经细胞）是读入输入数据的。只要你有数据，这层的神经元就能运行。中间则是”隐含层。“你可以控制这个隐含层的层数，以及每一层里有多少个神经元或者神经细胞。当然在实际操作里为了方便我们一般都直接认为你不管用几层，每层的神经元或者神经细胞数目都是一样的。因为这样的话写代码会比较方便。<br>
**神经网络模型的核心要务就是不断的调整权重，得出一组最接近完美答案的权重值**<br>
##### BP神经网络(Back Propagation)
BP神经网络就是在前馈型网络的结构上增加了后向传播算法（Back Propagation）。前馈只是用于计算出网络的输出，不对网络的参数进行调整,而[反向传播](https://blog.csdn.net/u014303046/article/details/78200010)的基本思想就是通过计算输出层与期望值之间的误差来调整网络参数，从而使得误差变小。该过程是需要监督学习的。在网络没有训练好的时候，输出肯定和想象的不一样，那么我们会得到一个`偏差`，并且把偏差一级一级向前传递，逐层得到δ<sup>(i)</sup>，这就是`反馈`。 反馈是用来求`偏导数`的，偏导数是用来作`梯度下降`的，梯度下降是为了求得`代价函数`的极小值，使得期望和输出之间的误差尽可能的减小。BP神经网络的算法流程为：<br>
![](https://github.com/yangxcc/Sklearn-Algorithm/blob/master/image/BP神经网络过程.png)<br>
大致可以分为5个步骤：
- 初始化网络权重以及偏置。我们知道不同神经元之间的连接权重（网络权重）是不一样的。这是在训练之后得到的结果。所以在初始化阶段，我们给予每个网络连接权重一个很小的随机数（一般而言为-1.0-1.0或者-0.5-0.5），同时，每个神经元有一个偏置（偏置可看做是每个神经元的自身权重)，也会被初始化为一个随机数。
- 进行前向传播。输入一个训练样本，然后通过计算得到每个神经元的输出。每个神经元的计算方法相同，都是由其输入的线性组合得到。
## Aprioir(不属于Sklearn算法，关联挖掘算法)
