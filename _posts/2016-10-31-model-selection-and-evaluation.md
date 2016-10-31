---
layout: post
title: 模型选择
description: 周志华之机器学习第二章
categories: 机器学习
tags: [模型选择,评价指标]
---

# 模型选择与评估

## 误差与过拟合
误差包括： 
 
* 训练误差(测试误差)  
* 泛化误差

我们希望的是泛化误差最小，但是无法直接获取泛化误差，所以一般用**测试误差**来近似泛化误差。  

**训练误差**太大导致欠拟合，太小导致过拟合。模型/参数选择不能通过训练误差来衡量。最理想的是通过泛化误差来进行模型选择，实际使用测试误差来近似。


## 模型选择
模型选择需要考虑的因素：  

* 泛化误差->测试误差  
* 时间开销  
* 空间（存储）开销  
* 可解释性

## 划分训练集测试集的方法

* 留出法 常用2/3~4/5
* 交叉校验(k-fold, k = 5,10,20) 特殊的留一法
* 自助法 放回抽样 数据集较小时可用，缺点是改变了数据分布

## 严格的数据划分

* 训练集
	* 训练集
	* 验证集 -> 用于模型选择和调参
* 测试集

模型和参数确定后，再用整个训练集重新训练模型，最后再测试集上给出测试误差，作为最终泛化误差的近似。

## 性能度量

### 回归任务
最常见的是**均方误差**（mean square error）  

<img src="http://www.forkosh.com/mathtex.cgi? E(f;D)=\frac{1}{m}\sum_{i=1}^{m}\left(f(\mathbf{x_i})-y_i\right)^2">

更一般的，  

<img src="http://www.forkosh.com/mathtex.cgi? E(f;\mathcal{D})=\int_{\mathbf{x} \sim \mathcal{D}}\left(f(\mathbf{x})-y\right)^2p(\mathbf{x})d\mathbf{x}">

### 分类任务

* 错误率/精度(accuracy)
* 查准率、查全率、F1

	真实\预测 	| T   | F
	----		| ---- | ----
	T		  	| TP  | FN
	F		  	| FP  | TN
	
	P(rcision) 	= TP/(TP+FP)  
	R(ecall) 		= TP / (TP+FN)  
	PR曲线平衡点，R=P时的值  
	
	F1 = (2\*P*R)/(P+R) ->调和平均数
	
	当对P或R有不同的侧重时：  
	<img src="http://www.forkosh.com/mathtex.cgi? F_\beta=\frac{(1+\beta^2)*P*R}{(\beta^2*P)+R}">  
	当beta<1时，P影响更大  
	
* ROC与AUC  
	ROC: Receiver Operating Characteristic  
	纵轴: 真正率  TPR = TP / (TP + FN)  
	横轴: 假正率  FPR = FP / (TN + FP)  
	
	![ROC](https://upload.wikimedia.org/wikipedia/commons/6/6b/Roccurves.png)
	
	绘制过程：从（0，0）点开始绘制到（1，1）正例阈值从最大到最小  
	假设真实有m+个正例m-个负例，设前一个点为(x,y)则下一个点：  
	* TP -> (x + 1/m+, y)
	* FP -> (x, y + 1/m-)  
	故有限个测试样例绘制ROC图时应该是**锯齿状**图
	
	AUC: Area Under ROC Curve 衡量**排序质量**
	估算为:  
	<img src="http://www.forkosh.com/mathtex.cgi? AUC=\frac{1}{2}\sum_{i=1}^{m-1}(x_{i+1}-x_i)*(y_i+y_{i+1})">  
	
#### 代价敏感错误与代价曲线
分错类别代价不等时 ROC->代价曲线

## 比较检验
由于

1. 测试误差不能完全反应泛化性能
2. 测试集有随机性
3. 机器学习算法本身有随机性  

可以利用**统计假设检验**来提供置信度依据  


## 偏差方差分解

算法的期望泛化误差:  

<img src="http://www.forkosh.com/mathtex.cgi? E(f;D) = E_D[(f(\mathbf{x};D) - y_D)^2] = bias^2(\mathbf{x}) + var(\mathbf{x}) + \epsilon^2">

即泛化误差可分解为偏差、方差、噪声之和  

* 偏差度量了学习算法的期望预测与真实结果的偏离程度，刻画了学习算法本身的拟合能力 
* 方差度量了同样大小的训练集的变动所导致的学习能力的变化，刻画了数据扰动所造成的影响
* 噪声表达了在当前任务上任何学习算法所能达到的期望泛化误差的下界，刻画了学习问题本身的难度  

随着训练程度的加大，偏差减小，方差加大，泛化误差先减小后增大。

![bias-variance-tradeoff](https://github.com/xiangcong/xiangcong.github.io/blob/master/images/bias-variance-tradeoff.png?raw=true)
 





  
	




	        




