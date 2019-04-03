---
layout: post
title: Going Deeper with Convolutions FL
tags: Notes CNN Classification NetStruc
categories: Paper
excerpt: GoogLeNet的提出文章，提出了使用Inception Module的想法
---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

### 原文链接：[Going Deeper with Convolutions (2015)](https://arxiv.org/pdf/1409.4842.pdf)

# Introduction

为了提高网络的深度，going deeper。By deeper，本文由两个含义，一个是字面意义上的更深（depth）；另一个是引入了一个组成单元“Inception module”

# 灵感和一些考虑

传统的加深加宽有两个问题：overfitting，或者需要更多labeled example which获取起来非常昂贵；极大的增加了所需的计算资源

一个通用的解决方案，**sparsity（？有论文）**但现有的计算体系对这些稀疏的计算处理起来极其缓慢

本文提出了一种新的结构“Inception module”，用dense的矩阵计算来近似sparsity，但尽管在本文的实验中表现较好，但其是否具有推广性和普适性还有待考究

# 结构的细节
![gn]({{"/googlenet.png" | prepend: site.imgrepo }})

auxiliary classifiers为了解决梯度消失问题，并且更好的针对中间层的信息，最终乘上一个discount weight加到总的loos中去。
但是他们被验证仅有很小的效果（less than 0.5%），且只需要一个去达到相同的效果

# 后续有许多针对Inception Module的结构的改进探索工作
\[1\][Inception-v4, Inception-ResNet and the Impact of Residual Connections on Learning (2016)](https://arxiv.org/pdf/1602.07261v1.pdf)
\[2\][Rethinking the Inception Architecture for Computer Vision (2016)](https://arxiv.org/pdf/1512.00567.pdf)