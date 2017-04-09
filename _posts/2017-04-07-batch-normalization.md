---
layout: post
title: batch normalization
description: bn
categories: 深度学习
tags: [dnn]
---
## Batch Normalization 

Batch Normalization allows us to:

* use much **higher learning rates** and 
* be **less careful about initialization**.
* It also acts as a **regularizer**, in some cases eliminating the need for Dropout

算法如下：

![bn](https://github.com/xiangcong/xiangcong.github.io/blob/master/images/batchNormalization.png?raw=true)

其中伽马和贝塔是需要学习的参数。

参考：https://www.zhihu.com/question/38102762
http://blog.csdn.net/happynear/article/details/44238541