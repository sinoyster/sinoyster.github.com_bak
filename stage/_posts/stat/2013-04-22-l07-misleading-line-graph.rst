---
layout: post
title: "第七课 misleading line graph 误导人的线形图"
description: "misleading line graph 误导人的线形图"
category: stat
tags: ["R", "公开课", "统计学"]
---
{% include JB/setup %}

[\ 第七课_\ ]  misleading line graph 误导人的线形图"

.. _第七课: http://v.163.com/movie/2011/6/S/E/M82IC6GQU_M83J9PESE.html


线形图优势在于同一副图中表达趋势发展，如果不同的线形图进行比对建议要有相同的起始刻度和比例，否则图像会失真, 实际上可以在一幅图中画多个线图进行比较

看教程中例子两幅图，Y坐标起始值不同，并且比例也差距很大

我们先用R分别画出两幅图，再把两幅图放一幅图中来比较

.. code:: 

  #数据准备
  yummy <- c(80, 76, 77, 72, 72, 68)
  thrill <- c(12, 19, 19, 20, 21, 25)
  duration <- c(2006:2011)
  
  #第一幅图
  plot(duration, yummy, type="l", col="red", ylim=c(50,100), ylab="%", main="Precentage of People Who Prefer Yummy Cola")
  #第二幅图
  plot(duration, thrill, type="l", col="blue", ylim=c(0,30), ylab="%", main="Precentage of People Who Prefer Thrill Cola")
  
  #第三幅图
  > plot(duration, yummy, type="l", col="red", ylim=c(0,100), ylab="%", main="Precentage of People Who Prefer  Cola")
  > lines(duration, thrill, col="blue")

第一幅图：

.. figure:: /assets/image/stat/stat_r/l07_demo_misleading_f1.png

第二幅图：

.. figure:: /assets/image/stat/stat_r/l07_demo_misleading_f2.png

第三幅图：

.. figure:: /assets/image/stat/stat_r/l07_demo_misleading_f3.png
