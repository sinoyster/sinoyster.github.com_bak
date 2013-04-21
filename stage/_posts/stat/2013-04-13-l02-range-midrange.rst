---
layout: post
title: "第二课 range midrange"
description: "range极差 midrange中程数 "
category: stat
tags: ["R", "统计学", "公开课"]
---
{% include JB/setup %}




[\ 第二课_\ ] 极差(range), 中程数(midrange)

.. _第二课: http://v.163.com/movie/2011/6/6/0/M82IC6GQU_M83J9IK60.html


1.range 极差
++++++++++++++

级差（range）：最大数减去最小数。数字越小级差越小。它描述这些数字分开的有多远, 差值越小，数据分布得越紧密

R中range用于生成向量，像这种最大减最少用下面的方法就可以实现

.. code::

  max(x)-min(x)

2.midrange 中程数
++++++++++++++++++++

中程数(midrange): 中程数是指数据集中最大数和最小数的平均值，是考虑集中趋势的又一种方式，是考虑中间值的有一种方法

.. code::

  (max(x) + min(x))/2

