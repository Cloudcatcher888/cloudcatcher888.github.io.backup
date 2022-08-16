---
layout: post
title:  "Multi-Interest Modeling"
date:   2022-08-10
categories: jekyll update
---
# 多兴趣序列建模

## 现有工作发表纪年：

* 2019  MIMN   基于NTM神经图灵机 KDD

  基于memory，每产生一条交互记录就会去attention地写多头memory，同时也会去attention地读多头memory（分为擦除和增加）以用于训练更新参数。上线以后多头memory就作为用户表征。

  缺点： cannothandle 10000 seq  This is because encoding all user historical behaviors into a fixed size memory matrix causes massive noise tobe contained in the memory units.

  ![image-20220816114541983]({{site.url}}/images/image-20220816114541983.png)

* 2019 HPMN Hierarchical RNN SIGIR

  用Hierachical RNN去生成multi scope的memory，读取memory依然保持NTM的方式。

  缺点：memory不是针对多兴趣，是对序列不同粒度的抽象，存在信息冗余的风险。

  ![image-20220816114449543]({{site.url}}/images/image-20220816114449543.png)

* 2019 SIM search base的方法

  先基于类别索引（或者物品相似度）检索出长序列中所有和target item同类的interated item，构成子序列，再看成短序列问题

  缺点：非常明显，没有把user representation的更新和target item解耦开来，不存在Ur的概念，也不存在增量更新Ur的可能，性能上不如其他解耦的方法。

  ![image-20220816114356130]({{site.url}}/images/image-20220816114356130.png)

* 2019 MIND 胶囊网络 CIKM

  利用胶囊网络解耦user representation和target item

  缺点：无法融合新的序列，有新的序列需要在整个序列上重新计算所有表征

  ![image-20220816114616543]({{site.url}}/images/image-20220816114616543.png)

* 2020 ComiRec 胶囊网络+自注意力模型 KDD

  利用胶囊网络或者自注意力模型解耦user representation和target item，相比MIND，增加了一些[Exploitation & Exploration](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/N3n7aegr6wYIhCF7yeSSSg)平衡的探索

  缺点：无法融合新的序列，有新的序列需要在整个序列上重新计算所有表征

  ![image-20220816114759418]({{site.url}}/images/image-20220816114759418.png)

* 2022 LimaRec 自注意力模型（线性版本）arxiv

  利用线性自注意力模型可以解耦user representation和target item，同时有新的序列也可以融合进user representation中，实现增量更新Ur。和MIMN和HPMN达到相同目的的前提下使用了表达能力更强的自注意力模型。

  ![image-20220816113937762]({{site.url}}/images/image-20220816113937762.png)
  
  ![image-20220816114151880]({{site.url}}/images/image-20220816114151880.png)
  
  | method  | incremental Ur update | long seq | multi interest   | decouple | retrain |
  | ------- | --------------------- | -------- | ---------------- | -------- | ------- |
  | MIMN    | yes                   | yes      | yes(memory)      | yes      | full    |
  | HPMN    | yes                   | yes      | yes(multi scope) | yes      | full    |
  | MIND    | no                    | yes      | yes(dr)          | yes      | full    |
  | SIM     | no                    | yes      | yes(sa)          | no       | full    |
  | ComiRec | no                    | yes      | yes(sa,dr)       | yes      | full    |
  | LimaRec | yes                   | yes      | yes(sa)          | yes      | full    |
  
  

## 核心思想：

1 前提是序列建模，主要目的是解决序列模型推断时耗时，存储开销大的问题。

2 核心是取消了DIEN在序列中计算attention，使得物品表征和用户表征的计算分离开来。

3  之所以引入多兴趣，是因为在上面的思想基础上，如果只用一个向量作为用户的表征太弱，并且用户天然具有多个兴趣，用多个向量表示也更为直观。

## 缺陷

多兴趣建模的过往工作始终没有解决多兴趣自适应更新（往大了说没考虑模型参数增量更新）的问题。如果只是简单地在一开始给一个很大的兴趣数K，以往工作也大多证明了在K非常大时模型性能反而会下降，说明这样粗暴的方式是行不通的。（需要过一段时间输入整段序列重新训练模型）