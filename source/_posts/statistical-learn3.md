---
title: 统计学习方法——k近邻算法
tags: 机器学习
mathjax: true
date: 2018-11-05 10:57:41
---


k近邻法(k-nearest neightbor,k-NN)是一种基本分类与回归方法.k近邻法的输入为实例的特征向量，对应于特征空间的点；输出为实例的类别，可以取多类.k近邻法假设给定一个训练数据集，其中的实例类别已定.分类时，对新的实例，根据其k个最近邻的训练实例的类别，通过多数表决等方法进行预测.因此，k近邻法不具有显式的学习过程.k近邻法实际上利用训练数据集对特征向量空间进行划分,并作为其分类的"模型".k值的选择、距离度量及分类决策规则是k近邻法的三个基本要素.k近邻法1968年由Cover和Hart提出.

<!-- more -->

## k近邻算法
**k近邻法**

输入：训练数据集$$T=\\{(x_1,y_1),(x_2,y_2),\cdots,(x_N,y_N)\\}$$其中，$x_i\in X = R^n$,$y_i\in Y = \\{c_1,c_2,\cdots,c_K\\},i=1,2,\cdots,N$;实例特征向量$x$;

输出：实例$x$所属的类$y$.

1. 根据给定的距离度量，在训练集$T$中找出与$x$最近邻的$k$个点，涵盖这$k$个点的$x$的邻域记作$N_k(x)$;
2. 在$N_k(x)$中根据分类决策规则(如多数表决)决定$x$的类别$y$:$$y=argmax \sum\limits_{x_i\in N_k(x)} I(y_i=c_j),i=1,2,\cdots,N;j=1,2,\cdots,K \tag{1}$$其中$I$为指示函数，即当$y_i=c_j$时$I$为1，否则$I$为0.

k近邻法的特殊情况是$k=1$的情形，称为最近邻算法.对于输入的实例点(特征向量)$x$,最近邻算法将训练集中与$x$最近邻的点作为x的类.

k近邻法没有显式的学习过程.

## k近邻模型
k近邻法使用的模型实际上对应于对特征空间的划分.模型由三个基本要素——距离度量、k值的选择和分类决策规则决定.

### 距离度量
特征空间中两个实例点的距离是两个实例点相似程度的反映.k近邻模型的特征空间一般是$n$维实数向量空间$R^n$.使用的距离是欧式距离，但也可以是其他距离，如更一般的$L_p$距离或Minkowski距离.

### k值的选择
k值的选择会对k近邻法的结果产生重大影响.

如果选择较小的k值，就相当于用较小的邻域中的训练实例进行预测，"学习"的近似误差会减小，只有与输入实例较近的训练实例才会对预测结果起作用.但缺点是"学习"的估计误差会增大，预测结果会对近邻的实例点非常敏感.k值的减小就意味着整体模型变得复杂，容易发生过拟合.

如果选择较大的k值，其优点是可以减少学习的估计误差.但缺点是学习的近似误差会增大.k值的增大意味着整体的模型变得简单.

在应用中，k值一般取一个比较小的数值.通常采用交叉验证法来选取最优的k值.

### 分类决策规则
k近邻法的分类决策规则往往是多数表决,即由输入实例的k个近邻的训练实例中的多数类决定输入实例的类.

多数表决规则有如下解释：如果分类的损失函数为0-1损失函数，分类函数为$$f:R^n\rightarrow \\{c_1,c_2,\cdots,c_K\\}$$那么误分类的概率是$$P(Y\neq f(X))=1-P(Y=f(X))$$对给定的实例$x\in X$，其最近邻的k个训练实例点构成集合$N_k(x)$.如果涵盖$N_k(x)$的区域的类别是$c_j$,那么误分类率是$$\frac{1}{k}\sum\limits_{x_i\in {N_k(x)}} I(y_i\neq c_j)=1-\frac{1}{k}\sum\limits_{x_i\in {N_k(x)}} I(y_i= c_j)$$要使误分类率最小即经验风险最小，就要使$\sum\limits_{x_i\in {N_k(x)}} I(y_i= c_j)$最大，所以多数表决规则等价于经验风险最小化.