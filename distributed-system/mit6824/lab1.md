---
title: MIT 6.824 - Lab 1 - MapReduce 学习笔记
description: MIT 6.824 Distributed System课程实验一：MapReduce学习笔记
published: true
date: 2021-06-25T19:18:41.883Z
tags: distributed-system, mit6.824, map-reduce, labs
editor: markdown
dateCreated: 2021-06-25T19:14:38.030Z
---

> MapReduce是Google的Jeff Dean大佬等人在2004年提出的大数据处理框架。  
> 它的基本原理是将数据拆分成多个块，在多个机器上并行处理。  
> 原本需要高性能计算资源的数据处理任务，通过MapReduce框架可以放到多个廉价的机器上并行处理。  

## MapReduce 原理纪要

MapReduce框架的整体框架运行原理分为如下几个步骤：

1. MapReduce框架总体分为一个`Master`和多个`Worker`，`Master`和`Worker`的作用分别如下：
   - **Master**：负责整个MapReduce集群的任务分配和调度工作；
   - **Worker**：负责执行被分配的**Map**或者**Reduce**任务。
2. MapReduce的输入为`M`个文件或者数据块，数据之间是没有任何依赖性的；输出`R`个Reduce结果文件；
3. 在`Map`任务阶段，每一个`Worker`会被分配有且仅有一个文件，对文件内容执行用户自定义的`Map`函数，此`Map`函数会输出若干`(Key, Value)`对，并保存至中间文件（在Paper中，此文件被统称为**中间结果**）；
4. 当`Map`任务全部执行完毕之后，`Master`分配`R`个`Reduce`任务，每一个`Worker`对应一个Reduce任务，收集该`Reduce`任务对应的所有中间结果，读取`(Key, Value)`对，将它们排序之后执行用户自定义的`Reduce`函数，最后将结果输出。



## Reference

- [MapReduce Paper on Google Research](https://research.google/pubs/pub62/)

