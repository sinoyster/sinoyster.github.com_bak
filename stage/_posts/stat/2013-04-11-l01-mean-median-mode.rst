---
layout: post
title: "第一课 mean median mode"
description: "mean median mode"
category: stat
tags: ["R", "公开课", "统计学"]
---
{% include JB/setup %}




第一课_  mean median 和 mode

.. _第一课: http://v.163.com/movie/2011/6/6/0/M82IC6GQU_M83J9IK60.html


1.mean 算术平均数
+++++++++++++++++++

算术平均，就是累计求和除以个数

R中用mean计算

用法:

::

  mean(x, ...)
  ## Default S3 method:
  mean(x, trim = 0, na.rm = FALSE, ...)

  trim： 一个0到0.5的因子，用于清除样本中偏离比例, 修剪值超出该范围的定为最接近的端点。
       通常用于去掉一些误差数据。注意，数据会在两端同时去掉
       the fraction (0 to 0.5) of observations to be trimmed from
       **each end** of ‘x’ before the mean is computed.  Values of trim
       outside that range are taken as the nearest endpoint.
       

  na.rm: 一个逻辑值，表明NA值是否会清除不参与计算
       a logical value indicating whether ‘NA’ values should be
       stripped before the computation proceeds.
       



trim例子:

.. code::

  > x=sort(rnorm(100,10,2))
  > mean(x)
  [1] 10.18032
  > 
  > c(mean(x[11:90]),mean(x,trim=.1))
  > [1] 10.20156 10.20156
  > 
  > c(mean(x[21:80]),mean(x,trim=.2))
  > [1] 10.24533 10.24533
  >
  >
  > x <- c(1:3,6)
  > mean(x)
  [1] 3
  
  > mean(x,0.25)
  [1] 2.5
  #实际上去掉了头和尾1和6两个数
  > mean(c(1:100,Na))
  [1] Na


2.median 中位数 
+++++++++++++++++

如果数量为偶数，取中间两个数的平均值; 数量为奇数就是中间数字的值

例子:

.. code::

  > median(c(1:4),10)
  [1] 2.5

3.mode 众数 
++++++++++++++

众数：出现频率最多的数字

R中没有直接实现众数的函数，另外R中的mode也有另外的含义，不过可以用变通的方法实现

.. code::

  which.max(table(x))

例子:

.. code::

  > x1 <- c(1:10,3)
  > x1
   [1]  1  2  3  4  5  6  7  8  9 10  3
  > which.max(table(x1))
  3 
  3 
  
