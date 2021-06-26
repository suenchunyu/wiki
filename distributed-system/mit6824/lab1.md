---
title: MIT 6.824 - Lab 1 - MapReduce 学习笔记
description: MIT 6.824 Distributed System课程实验一：MapReduce学习笔记
published: true
date: 2021-06-26T09:31:20.276Z
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

![map-reduce.png](/map-reduce.png)

以**词频计数**这个场景为例，它的Map和Reduce函数可以如下表示：

```
Example: word count
	input is thousands of text files
  
  Map(key, val)
  	split v into words
    for each word w
    	emit(w, "1")
  
  Reduce(key, val)
  	emit(len(val))
```

## 需求分析

根据Lab中的描述，可以总结出如下几个待实现的需求点

1. 我们有且仅有一个`master`实例，它需要来负责分配调度`MapReduce`任务；
2. `worker`实例需要不断的向`master`请求任务，在完成任务之后需要向`master`报告任务已经完成；
3. `master`先分配`Map`任务，必须在所有的`Map`任务都已经被分配且结束之后，才可以继续分配`Reduce`任务；
4. `master`在完成所有的任务之后退出，此时如果还有`worker`向`master`请求任务，会因为RPC调用失败而直接退出；
5. 如果`worker`意外退出，或者长时间出现未响应的状况，`master`要重新分配对应`worker`的任务。

## 实现思路

首先整个MapReduce框架中主要的对象是`Worker`和`Master`，通过需求分析阶段和Lab中可针对`Master`总结如下几点：

1. `Master`需要整理并维护所有输入的

## Reference

- [MapReduce Paper on Google Research](https://research.google/pubs/pub62/)

- [MIT 6.824 Lab 1 - Map Reduce](https://pdos.csail.mit.edu/6.824/labs/lab-mr.html)


