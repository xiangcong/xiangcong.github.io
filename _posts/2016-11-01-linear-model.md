# 线性模型

优点：  

* 简单
* 可解释性好

	<img src="http://www.forkosh.com/mathtex.cgi? y=w^Tx">   
=>  
	<img src="http://www.forkosh.com/mathtex.cgi? \hat{w}=(X^TX)^{-1}X^TY">  	
	=>  
		<img src="http://www.forkosh.com/mathtex.cgi? \hat{Y}=X(X^TX)^{-1}X^TY">  
		
#### 线性回归
<img src="http://www.forkosh.com/mathtex.cgi? 
y = w^Tx+b">
#### 对数线性回归
<img src="http://www.forkosh.com/mathtex.cgi? lny=w^Tx+b">  

形式上是线性，实质上已经是在求输入空间到输出空间的非线性映射  

#### 广义线性模型
<img src="http://www.forkosh.com/mathtex.cgi? y=g^{-1}(w^Tx+b)">  

其中 g(.)是单调可微函数

## 对数几率回归

#### 将线性回归模型应用于分类
需要找一个合理的g(.)，将分类任务的真实标记与线性回归模型的预测值联系起来  

最简单的是**阶跃函数**（对于二分类问题):  
<img src="http://www.forkosh.com/mathtex.cgi? 
y=
\begin{cases}
0& x<0\\
0.5& x=0\\
1& others
\end{cases}  
">

但是阶跃函数不连续 -> 不可微 -> 寻找替代函数 -> 对数几率函数:
<img src="http://www.forkosh.com/mathtex.cgi? 
y=
\frac{1}{1+e^{-w^Tx}}
">  

![sigmod](https://upload.wikimedia.org/wikipedia/commons/8/88/Logistic-curve.svg)  

y可以看做是样例是正例的可能性  
y/(1-y)作为正例的相对可能性，称为“几率”  

**对数几率：**  

<img src="http://www.forkosh.com/mathtex.cgi? 
ln\frac{y}{1-y}=
w^Tx}
">   

用线性回归模型的预测结果去逼近真实标记的对数几率 -> 对数几率回归  
**优点：**  

* 无需事先假设数据分布
* 不仅预测类别，还得到了近似概率预测
* 对数函数是任意阶可导的凸函数

## 极大似然估计  
对数似然 ：  

<img src="http://www.forkosh.com/mathtex.cgi? 
l(w,b)=
\sum_i lnp(y_i|x_i;w,b)}
">  

将w,b统一成beta，并修改x(追加1)  

<img src="http://www.forkosh.com/mathtex.cgi? \Large 
l(\beta)=\sum_i ln(p_1^{y_i}p_0^{1-y_i})
=\sum_i (y_ilnp_1 + (1-y_i)lnp_0))
=\sum_i (y_iln(p_1/p_0)+lnp_0)
=\sum_i(y_i\bate^Tx_i - ln(1-\exp(\beta^Tx_i)))}
">  

最大化上式，等价于最小化下式  

<img src="http://www.forkosh.com/mathtex.cgi? 
l(\beta)
=\sum_i(-y_i\bate^Tx_i + ln(1-\exp(\beta^Tx_i)))}
">  
是一个关于\beta的高阶可导连续凸函数，可用数值优化算法如：梯度下降、牛顿法等求解 

<img src="http://www.forkosh.com/mathtex.cgi? 
\beta^* = \arg\min_\beta l(\beta)
">  

## 线性判别分析 LDA

**基本思想：**将高维输入映射到低维空间，使得同类的投影点尽可能近，异类的投影点尽可能远

### 两类的情况
令 
<img src="http://www.forkosh.com/mathtex.cgi? \Small \mu_i, \Sigma_i, i\in\{0,1\}">  分别表示第i类示例的均值向量，协方差矩阵。若将数据投影到直线w上，则两类样本中心点在直线上的投影为
<img src="http://www.forkosh.com/mathtex.cgi? w^T\mu_0, w^T\mu_1">。
若讲所有样本都投影到直线上，则两类样本的协方差分别为
<img src="http://www.forkosh.com/mathtex.cgi? w^T\Sigma_0w, w^T\Sigma_1w">。


根据LDA的基本思想，可以让同类投影点的协方差尽可能小，而异类中心点距离尽可能大，同时考虑两者，可得到如下最大化目标：

<img src="http://www.forkosh.com/mathtex.cgi? J = \frac{||w^T\mu_0 - w^T\mu_1||_2^2}{w^T\Sigma_0w + w^T\Sigma_1w} = \frac{w^T(\mu_0-\mu_1)(\mu_0-\mu_1)^Tw}{w^T(\Sigma_0+\Sigma_1)w}">  

定义**类内散度矩阵**(within-class scatter matrix)  

<img src="http://www.forkosh.com/mathtex.cgi? S_w=\Sigma_0+\Sigma_1=\sum_{x\in\mathbf{X}_0}(x-\mu_0)(x-\mu_0)^T + \sum_{x\in\mathbf{X}_1}(x-\mu_1)(x-\mu_1)^T">  

以及**类间散度矩阵**(between-class scatter matrix)  

<img src="http://www.forkosh.com/mathtex.cgi? S_b=(\mu_0-\mu_1)(\mu_0-\mu_1)^T">  

则J可重写为：  

<img src="http://www.forkosh.com/mathtex.cgi? J = \frac{w^TS_bw}{w^TS_ww}">  

由拉格朗日乘子法及w扩大任何倍都不会影响可得:(省略了推倒公式)

<img src="http://www.forkosh.com/mathtex.cgi? w=S_w^{-1}(\mu_0-\mu_1)">  


### 推广到多类的情况

可参考这篇[blog](http://www.cnblogs.com/jerrylead/archive/2011/04/21/2024384.html)，写的很不错。

假设现在有N个类(N>2)，如果还是映射到1维空间，显然不足以分类。需要映射到K维(结论显示K最大为N-1,通过矩阵的秩获得)  

类内散度矩阵与之前定义类似

<img src="http://www.forkosh.com/mathtex.cgi? S_w = \sum_{i=1}^NS_{w_i}">  

<img src="http://www.forkosh.com/mathtex.cgi? S_{w_i} = \sum_{x\in\mathbf{X_i}}(x-\mu_i)(x-\mu_i)^T">   

类间散度矩阵需要改写成各类中心与样本中心的距离，同时考虑权重(该类样本的个数)

<img src="http://www.forkosh.com/mathtex.cgi? S_b = \sum_{i=1}^{N}m_i (\mu_i-\mu)(\mu_i-\mu)^T">  

其中m_i表示第i类样本的个数，mu是所有样本的均值  

但是LDA考虑的是映射后的空间，假设W为映射矩阵(d*N-1) W'x 即为映射后的向量(N-1维)

优化目标为:  

<img src="http://www.forkosh.com/mathtex.cgi? \arg\max_W\frac{tr(W^TS_bW)}{tr(W^TS_wW)}"> 

进而转化为求解:  

<img src="http://www.forkosh.com/mathtex.cgi? S_bW=\lambda S_wW">  

W的闭式解为<img src="http://www.forkosh.com/mathtex.cgi? S_w^{-1}S_b">的N-1个最大广义特征值所对应的特征向量组成的矩阵  

由于将样本投影到了N-1维空间，往往小于数据原有的属性数，因此LDA也被视为一种经典的监督降维技术。  

## 多分类学习

1. 一对一
2. 一对其余
3. 多对多 纠错输出码  

## 类别不均衡  

1. 欠采样
2. 过采样
3. 阈值移动 
  

  






	

	
