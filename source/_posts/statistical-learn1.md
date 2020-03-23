---
title: 统计学习方法概论
tags: 机器学习
date: 2018-09-06 09:18:48
---


1. 统计学习是关于计算机基于数据构建概率统计模型并运用模型对数据进行分析与预测的一门学科。统计学习包括监督学习、非监督学习、半监督学习和强化学习。
2. 统计学习方法三要素——模型、策略、算法。
3. 监督学习可以概括如下：从给定有限的训练数据出发，假设数据是独立同分布的，而且假设模型属于某个假设空间，应用某一评价准则，从假设空间中选取一个最优的模型，使它对已给训练数据及未知测试数据在给定评价标准意义下有最准确的预测。
4. 统计学习中，进行模型选择或者说提高学习的泛化能力是一个重要问题。如果只考虑减少训练误差，就可能产生过拟合现象。模型选择的方法有正则化与交叉验证。学习方法泛化能力的分析是统计学习理论研究的重要课题。
5. 分类问题、标注问题和回归问题都是监督学习的重要问题。统计学习方法有感知机、k近邻法、朴素贝叶斯法、决策树、逻辑斯谛回归与最大熵模型、支持向量机、提升方法、EM算法、隐马可夫模型和条件随机场等。这些方法是主要的分类、标注以及回归方法。它们又可以归类成生成方法与判别方法。

<!-- more -->
## 统计学习
### 统计学习的特点
1. 统计学习以计算机及网络为平台，是建立在计算机及网络之上的。
2. 统计学习以数据为研究对象，是数据驱动的学科。
3. 统计学习的目的是对数据进行预测与分析。
4. 统计学习以方法为中心，统计学习方法构建模型并应用模型进行预测与分析。
5. 统计学习是概率论、统计学、信息论、计算理论、最优化理论及计算机科学等多个领域的交叉学科，并且在发展中逐步形成独自的理论体系与方法论。

**学习**：<u>如果一个系统能够通过执行某一过程改进它的性能，这就是学习。</u>

**统计学习**：<u>计算机系统通过运用数据及统计方法提高系统性能的机器学习。</u>

### 统计学习的对象
数据

关于数据的基本假设是同类数据具有一定的统计规律性。

以变量或变量组表示数据。数据分为由连续变量和离散变量表示的类型。


### 统计学习的目的

对数据进行预测与分析，特别是对未知新数据进行预测与分析。

通过构建概率统计模型实现。

考虑学习什么样的模型和如何学习模型，以使模型能对数据进行准确的预测与分析，同时也要考虑尽可能地提高学习效率。


### 统计学习的方法

基于数据构建统计模型从而对数据进行预测与分析。
统计方法由监督学习、非监督学习、半监督学习和强化学习等组成。

监督学习：从给定的、有限的、用于学习的训练数据集合出发，假设数据是独立同分布产生的；并且假设要学习的模型属于某个函数的集合，称为假设空间；应用某个评价准则，从假设空间中选取一个最优的模型，使它对已知训练数据及未知测试数据再给定的评价准则下有最优的预测；最优模型的选取由算法实现。

统计学习方法包括**模型的假设空间**、**模型选择的准则**以及**模型学习的算法**，称其为统计学习方法的三要素，**模型**、**策略**和**算法**。

统计学习方法步骤：

1. 得到一个有限的训练数据集合；
2. 确定包含所有可能的模型的假设空间，即学习模型的集合；
3. 确定模型选择的准则，即学习的策略；
4. 实现求解最优模型的算法，即学习的算法；
5. 通过学习方法选择最优模型；
6. 利用学习的最优模型对新数据进行预测或分析。

## 监督学习

监督学习的任务是学习一个模型，使模型能够对任意给定的输入，对其相应的输出作出一个好的预测。

### 基本概念

#### 输入空间、特征空间与输出空间

输入与输出所有可能取值的集合分别称为输入空间与输出空间。

每个具体的输入是一个实例，通常由特征向量表示，所有特征向量存在的空间称为特征空间。特征空间的每一维对应于一个特征。模型实际上都是定义在特征空间上的。

输入变量与输出变量均为连续变量的预测问题称为回归问题；输出变量味有限个离散变量的预测问题称为分类问题；输入变量与输出变量均为变量序列的预测问题称为标注问题。

#### 联合概率分布
监督学习假设输入与输出的随机变量X和Y遵循联合概率分布P(X,Y)。

#### 假设空间
模型属于由输入空间到输出空间的映射的集合，这个集合就是假设空间。假设空间的确定意味着学习范围的确定。

监督学习的模型可以是概率模型或非概率模型。由条件概率分布P(Y|X)或决策函数Y=f(X)表示。

## 统计学习三要素

方法=模型+策略+算法

### 模型
监督学习过程中，模型是所要学习的条件概率分布或决策函数，模型的假设空间包含所有可能的条件概率分布或决策函数。

### 策略

统计学习的目标在于从假设空间中选取最优模型。

损失函数和风险函数，损失函数度量模型一次预测的好坏，风险函数度量平均意义下模型预测的好坏。

#### 损失函数和风险函数

损失函数或者代价函数用来度量预测错误的程度。损失函数是f(X)和Y的非负实值函数，记作L(Y,f(X))。
统计学习常用的损失函数：
1. 0-1损失函数
2. 平方损失函数
3. 绝对损失函数
4. 对数损失函数或对数似然损失函数