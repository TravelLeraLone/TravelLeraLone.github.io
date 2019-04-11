---
layout: post
title: 零示例学习中的一些概念和理解的总结
tags: Notes ZSL 总结
categories: Paper
excerpt: 在学习和实践过程中对于一些重要的知识点进行的一些能够帮助我自己理解的总结和归纳，很多是按我自己的思路进行的，所以并不一定完善也不一定真的包含一些内容，甚至不一定能够抓住其中的重点，所以仅做参考，同时欢迎大家批评指正！
---

* TOC
{:toc}

在学习和实践过程中对于一些重要的知识点进行的一些能够帮助我自己理解的总结和归纳，很多是按我自己的思路进行的，所以并不一定完善也不一定真的包含一些内容，甚至不一定能够抓住其中的重点，所以仅做参考，同时欢迎大家批评指正！


## 对嵌入的分类和理解

TODO



## 对属性层的调整总结

在学习与实践过程中，发现零示例学习的改进中有很大一部分是在对其属性层的结构或组成做文章，例如改进空间结构、加入限制条件、增加额外信息等，故而在此进行了一些总结。另，总结是基于本人看到的文章进行，并不保证完整完善。



想要更多的了解零示例学习，见：[总结：零次学习的简略发展历程（待完善）](https://travelleralone.github.io/2019-03-30/ZSL/)



---

1. 不处理或直接线性处理：在比较早期的工作中，许多工作都集中在怎样更好的将特征联系到属性中去，而对于属性并没有过多的关注，更多的只是简单的稍作处理即投入使用。

2. [基于流型结构的改造](https://arxiv.org/pdf/1703.05002.pdf)：该文章发现，不同的属性设定或不同的嵌入方法对最终结果有较大的影响，而作者认为流型结构的不同在其中起到了很大的作用。于是作者提出通过协同优化从特征到属性空间的映射$f​$和属性空间$\mathcal K​$（文中也叫语义嵌入空间）来使后者的流型结构达到最佳

   ![Algor1]({{ "/Algor1.png" | prepend: site.imgrepo }})

   其中，$\mathcal D_s = \{x_i,k_i,y_i\}^n_{i=1}$；$Eq.3: \arg \min_W (l(WX,K)+\gamma g(W))$。

3. [基于语义关系的改造](https://arxiv.org/pdf/1803.03049.pdf)：本文想要进一步利用属性中所含的语义信息。作者根据各个类别的属性向量，定义了不同类别之间的相似度测度：

   $$
   \delta_{m,n}=s(y_m,y_n)=\frac {<y_m,y_n>} {||y_m||_2||y_n||_2}
   $$
   

   并且定义对于两个类别（实际上是指对应的属性向量，后文中提到类别不明确说类别标签的情况下均指该类别的属性向量）$y_m,y_n$，$\delta_{m,n}=1$为相似关系，$\tau\leq\delta_{m,n}<1$为相似关系，其余为不相似关系。（其中$\tau\in(-1,1)$为超参数）

   同时本文选用了特征空间作为嵌入空间（即将属性向量嵌入到特征空间中）以缓解枢纽点问题，所以作者希望能够将这样的关系保持到嵌入空间中去。为了实现这一目标，对每个类别$y_r$，每次考虑一个三元组 $(x_i, x_j, x_k)$，其对应的类别 $(y_i, y_j, y_k)$ 分别与$y_r$是相同关系、相似关系、不相似关系。而考察的目标函数为：$(\mathcal B - size\ of\ minibatch;[z]_+=\max(0,z);f,g分别为编码器和解码器)$

   
   $$
   O=\frac1{|\mathcal B|}\sum_\mathcal B(O_1+\lambda_1O_2+\lambda_2O_3) \\
   O_1=\min_{\theta_f}-s(f(y_r;\theta_f),x_i)+(\tau-\delta_{k,r})s(f(y_r;\theta_f),x_k) \\
   O_2=\min_{\theta_f}[\tau-s(f(y_r;\theta_f),x_j)]_++[s(f(y_r;\theta_f),x_j)-\delta_{j,r}]_+ \\
   O_3=\min_{\theta_f,\theta_g}||y_r-\widehat y_r||^2_2
   $$
   

   另外：三元组的取样也是本文的重点之一。

4. [层级式隐藏属性学习](http://openaccess.thecvf.com/content_ICCV_2017/papers/Jiang_Learning_Discriminative_Latent_ICCV_2017_paper.pdf)：本文想要在保持一定的语义关系的情况下，解决用户定义的属性区别性不强、存在相关性（如女性和长发有一定的相关性）、内部方差较大（如老虎的尾巴和兔子的尾巴）的问题。作者提出用字典学习的方法，从用户定义的属性中学习生成新的拜托上面三个问题的隐藏属性：

   
   $$
   \arg \min_{D,Y_s,W,U}||X_s-DY_S||_F^2+\alpha||Y_s-WA_s||_F^2+\beta||H-UY_s||_F^2\\
   s.t.\ ||d_i||_2^2\leq1,\ ||w_i||_2^2\leq1,\ ||u_i||_2^2\leq1,\ \forall i \\
   where\ D-dictionary;Y-reconstruction\ coefficient;H-class\ label
   $$
   

   优化过程为：

   ![Algor2]({{ "/Algor2.png" | prepend: site.imgrepo }})

   在测试阶段，未见类别的隐藏属性值可以如下生成：

   
   $$
   a^u=\min_{a^u}||y^u-Wa^u||_F^2+\lambda||a^u||_2^2
   $$
   

5. [平行式隐藏属性学习](http://arxiv.org/pdf/1803.06731)：~~在本文之前的16年有一篇和层级式一样采用字典学习的方法进行这个问题的研究的，但该方法正如上文一样需要迭代收敛六组参数，较为复杂。（[链接](https://link.springer.com/content/pdf/10.1007%2F978-3-319-46493-0_21.pdf)）~~本文实际上做了多处创新，在对属性层的处理上，本文提出人工定义的属性可能并不完善且区别性不强，于是本文想要通过额外学习另一部分隐藏属性来补充人工定义的属性的不足。

   对于由卷积神经网络提取出的特征$\phi(x)$，施加一个线性变换$W^T_{aug}\in\mathbb R^{d\times2k}$，得到属性向量$\phi_e(x)=[\phi_{att}(x);\phi_{lat}(x)];\ \phi_{att}(x),\phi_{lat}(x)\in\mathbb R^k$。对于其中的用户定义属性向量$\phi_{att}(x)$，可以和传统的零示例学习类似的采用softmax损失函数来训练：

   
   $$
   \mathcal L_{att}= -\frac1N\sum_i^n\log\frac{exp(<\phi_{att}(x),a>)}{\sum_cexp(<\phi_{att}(x),a^c>)},\ c\in\mathcal Y_s
   $$
   

   而对于隐藏属性$\phi_{lat}(x)$为了让他能够有尽量强的区分能力，本文采用了以下损失函数：

   
   $$
   \mathcal L_{lat}=\max(0,m+d(\phi_{lat}(x_i),\phi_{lat}(x_k))-d(\phi_{lat}(x_i),\phi_{lat}(x_j)))
   $$
   

   其中：$x_i,x_k$同类别，$x_j$与它们不同类别。

   在测试阶段，为了能够生成未见类别的隐藏属性的标准值，本文做了一个假设：类别的属性向量在用户定义属性空间中的关系与在隐藏空间中的关系相同。基于这个假设，本文采取以下两个公式对未见类别的隐藏属性标准值进行生成：

   
   $$
   \forall u\in\mathcal Y_u \\
   \beta_c^u=\arg\min||a^u-\sum\beta^u_ca^c||^2_2+\lambda||\beta_c^u||_2^2,\ c\in\mathcal Y_s \\
   \overline {\phi^u_{lat}} = \sum\beta^u_c\overline{\phi^c_{lat}},\ c\in\mathcal Y_s
   $$
   

   随后便可以在两个空间或结合空间（指$\phi_e$的空间）中用余弦相似度进行分类。