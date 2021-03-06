****************
深度网络拓扑结构
****************

.. graphviz::
   
   digraph G {
      a [label="初始层"]
      b [label="隐层1（中层特征：边缘？？）"]
      c [label="隐层2（高层特征：shape？）"]
      d [label="决策层"]
      a->b  [label="卷积层"]
      c->b  [label="auto-coder"]
      b->d  [label="跨层网络实现多尺度特征"] 
      b->c  [label="使用sparse coding层层抽象"]
      c->d  [label="全局特征决策"]
   }


神经元结构
==========

#. 神经元从结构功能上来说：包括感知神经元和激活神经元。
#. 从感知神经元包括：线性的，卷积，限制玻尔兹曼机。
#. 激活函数包括（只要是连续即可）：sigmod，tanh，softmax，retified linear 。


每一种结构都是基于数学推理或者仿生学的原理。
并且网络能够自动的组成连接的拓扑，这才是人脑的过程。并且是动态的。
而现在的网络是静态的过程。

并且根据问题本身的模型来选择不同拓扑网络，现在还没有成熟的理论，只是经验性的应用。并且神经网络不会只有一种。也会需要各种类型。
并且现在神经元的构造是简单的运算来实现。是不是可以利用复杂的计算模型来构造神经元。
例如，在知道服从什么样分布的情况下，比如高斯分布，采用确定性的误差。在知道概率的情况下，使用最大似然或者熵进行推导，在概率也不知道的情况下，使用能量模型进行推导。 
神经元，激活函数，误差函数，由误差到参数loss函数，

随着深度网络增加，参数呈线性增加，但对问题描述能力成指数增长。并且神经网络本身也可以用来拟合复杂的复变函数公式的。 更因为这一拟合功能，把问题统一化了，有统一的解决方式。而不再是以前不断细化与breakdown的解决问题模式。我们就可以问题的颗粒度提高DL这一层了。剩下就交给机器去计算了。深度学习也算是一个算法数据结构的大一统。

层数不够的话，就要抽象逻辑不够，并且爆仓的问题。 也就是十个苹果放在九个篮子里。当然现在容量的研究还是空白。

网络的深度与每一个神经元的本身的复杂就是一个矛盾。每一个神经元简单，就需要更多层的网络。
复杂的神经元就可以实现更少层数来设计。


从级数的角度理论，深度是可以拟合一个函数的，精度误差，只与网络的级数相关。就像PCA算法的要求时。证明在这里http://neuralnetworksanddeeplearning.com/chap4.html。这也是深度网络的泛化能力的体现。 例如把tanh的级数展开式:http://www.wolframalpha.com/input/?i=sum+x%5E%282n-1%29%2F%282n-1%29%2C+n%3D1..oo
应该是一个傅里叶级数的变种。


但是反过来求网络所构成非线性函数在理论上是可能的，但是高维的计算量复杂。

我们能否把心理学试验与DL来进行对比实验。就像认知心理学中研究的XOR学习任务一样。从XOR分类任务看人类类别学习能力的局限性.pdf

把函数式编程+多态，是不是可以很方便实现神经网络。

网络框架
========

#. 基于限制波尔兹曼机

   - 卷积限制玻欠兹曼机
   - 三阶因子玻尔兹曼机

#. 基于自编码

   - 去噪自编码器
   - 变换自编码器

#. 卷积神经网络
    
   - 卷积神经网络
   - 卷积分解神经网络

.. include:: /Stage_2/toplogyStructure/cnn.rst
.. include:: /Stage_2/toplogyStructure/GAN.rst
.. include:: /Stage_2/toplogyStructure/RBM.rst
.. include:: /Stage_2/toplogyStructure/SVM.rst
.. include:: /Stage_2/toplogyStructure/SOM.rst
.. include:: /Stage_2/toplogyStructure/CelluarRC.rst
.. include:: /Stage_2/toplogyStructure/rl.rst



泛函网络
========

它在种个神经元之间的连接没有权值，并且神经元函数不是固定的，而是可学习的，通常是一个给定的基函数族的集合。可根据特定的问题，选择不同基函数。
基于泛函网络的多维函数逼近理论及学习算法.pdf


网络的层数与结构是根据问题的模型来的，盲目增加层数，就会使过拟合问题更加严重。太少又不足以表达。应该变成一个动态的网络。：:
