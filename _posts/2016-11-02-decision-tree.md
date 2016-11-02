---
layout: post
title: 决策树
description: 周志华之机器学习第四章
categories: 机器学习
tags: [机器学习,决策树]
---
# 决策树

每个节点代表具有特定属性特征的样本集。  
分支表示在一个属性上的划分。  
从父节点到子节点属性越来越具体，<mark>**纯度**</mark>越来越高，样本数越来越少。  

生成树的过程是一个递归的过程。  
3种情况下会导致递归终止：  

1. 当前节点所有样本的类别相同
2. 当前节点所有样本的属性相同
3. 当前节点的样本数为0  

但是上述严格的递归终止条件会导致比较差的泛化性能。可以考虑（也可看做是预剪枝）： 

1. 当一个节点上的样本数少于一个阈值
2. 再次划分后的纯度（信息增益等）小于某个阈值 



## 划分选择  

### 信息增益 ID3

信息熵：  

<img src="http://www.forkosh.com/mathtex.cgi? 
Ent(D) = -\sum_k p(k)ln(p(k))
">  
其中k为类别数

熵越大，无序性越大，纯度越小。故信息熵越小越好。  

信息增益：  

<img src="http://www.forkosh.com/mathtex.cgi? 
Gain(D,a) = Ent(D) - \sum_{v=1}^{V}\frac{|D_v|}{|D|}Ent(D_v)
">  
其中V为属性a上的属性值个数。  

根据信息增益来选择划分的属性，可能导致的问题是对可能取值较多的属性有所偏好。  

### 信息增益率 C4.5  

增益率：  

<img src="http://www.forkosh.com/mathtex.cgi? 
Gain_ratio(D,a) = \frac{Gain(D,a)}{IV(a)}
">  

其中IV(a)称为属性a的“固有值”：  

<img src="http://www.forkosh.com/mathtex.cgi? 
IV(a)=-\sum_{v=1}^{V}\frac{|D^v|}{D}log_2\frac{|D^v|}{|D|}}
">  

属性a的可能取值数目越多（即V越大），则IV(a)的值通常会越大。  

注意：增益率准则可能对可取值数目较少的属性有所偏好，因此，C4.5算法那并不是直接选择增益率最大的候选划分属性，而是使用了一个启发式：先从候选划分属性中找出信息增益高于平均水平的属性，再从中选择增益率最高的。  

### 基尼系数 CART  


<img src="http://www.forkosh.com/mathtex.cgi? 
Gini(D) = \sum_{k=1}^{|\mathcal{Y}|}\sum_{k'\neq k}p_kp_{k'} = 1 - \sum_{k=1}^{|\mathcal{Y}|}p_k^2
">   

Gini(D)反应了从数据集D中随机抽取两个样本，其类别标记不一致的概率，因此Gini约小，纯度越大。  

<img src="http://www.forkosh.com/mathtex.cgi? 
Gini\_index(D, a) = \sum_{v=1}^{V}\frac{|D^v|}{|D|}Gini(D^v)
">  

选择基尼指数最小的属性作为最有划分属性。  

## 剪枝处理  

利用测试集验证泛化性能。

防止过拟合：  

1. **预剪枝** 基于贪心，自顶向下，显著减少训练时间和测试时间，防止过拟合，有可能导致欠拟合
2. **后剪枝** 训练完之后，自底向上， 欠拟合风险小，时间开销大  

## 连续与缺失值  

### 连续值处理  

基于二分法（C4.5）  
对信息增益稍加改造：  

<img src="http://www.forkosh.com/mathtex.cgi? \Large 
Gain(D,a)=\arg\max_{t\in T_a}Gain(D,a,t) = \arg\max_{t\in T_a} Ent(D) - \sum_{\lambda \in \{-,+\}}\frac{|D_t^\lambda|}{|D|}Ent(D_t^\lambda)
">  

### 缺失值

1. 为每个样本分配权重
2. 在选择划分属性时，仅考虑属性没有缺失的
3. 在划分属性时，缺失属性的样本同时进入多个分支，同时调整样本权重

C4.5采用了上述算法

## 多变量决策树  

决策树的特点：**分类边界轴平行**  

非叶子节点不再是对特定的某个属性进行划分选择，而是对属性的线性组合进行测试  

## 回归树

类似聚类法，考虑“总体方差”(找不到一个合适的词)S，而非考虑信息增益等  

<img src="http://www.forkosh.com/mathtex.cgi? 
S = \sum_{c\in leaves(T)}n_cV_c
">  

其中  

<img src="http://www.forkosh.com/mathtex.cgi? 
V_c = \sum_{i\in c}(y_i - m_c)^2
">  

其中  

<img src="http://www.forkosh.com/mathtex.cgi? 
m_c = \frac{1}{n_c}\sum_{i\in c}y_i
">  

选择划分属性及划分点（如果是连续值属性）使得S下降的最多
