RNN
===

RNN 解决了DL中时间先后依赖的问题。 但是基本RNN只能是很近到远这种固定的依赖。
RNN只是提出了极数的形式。
LSTM解决了，就像极数，并且可以控制每级的参数。

又向LSTM解决了，前向依赖的问题，之前的问题只解决了后项依赖。 而对于attention 机制就像capsule中的sqush函数一样。

注意力机制 其实加权平均，以及压缩变换都是，是不是利用压缩变换再加6signa原则，直接发截判就够了。 
也就是输出要根据的输入进行动态的加权输出。 http://www.wildml.com/2016/01/attention-and-memory-in-deep-learning-and-nlp/,  
残差网直接把输入加入输出，而attension机制，则是把输入加权进入输出。

更进一步那就是注意力函数，来实现自由的挑选。 
`递归神经网络不可思议的有效性 <http://blog.csdn.net/mydear_11000/article/details/52414783>`_ 可以根据库来生成一个新序列。

例如你给一个名人的词句库，然后他能给你生成一个新名言了。
还有人直接用linux kernel代码让其学习，然后再生成代码。

对于单词训练，采用x,为目标，而x+1为label.

RNN,根据记忆的深度可以分为n-gram模型。

实现模型，因为数值计算量大，采用id,table式，在实现上，并且不是每一个神经元一个对象，而是所有输入是一个对象，所有的W值是一个对象，所有的B值是一个对象。
层层之间的连接关系，是通过计算反馈来实现的，而神经元之间的直接关系是通过W值来反映的。所以要清楚，哪是通过值反映的，哪些是通过计算反映的。

http://lib.csdn.net/article/deeplearning/45502, 

不仅仅有依赖过去，还有依赖未来的双向RNN。RNN一个关键是如何计算反馈上。

Vanilla RNN简单的向后看几步。
http://x-algo.cn/index.php/2016/04/25/rnn-recurrent-neural-networks-derivation-and-implementation/

Sequence To Sequences Model用于机器翻译。


LSTM,就是改变了RNN的结构，http://www.csdn.net/article/2015-01-28/2823747
其实就相当于一个三态门的开关。 http://lib.csdn.net/article/deeplearning/45652 公式的推导

http://lib.csdn.net/article/deeplearning/45392，实现了一个RNN来学习加法器。


对于语言处理的n-gram模型，http://blog.csdn.net/xiaokang06/article/details/17965965


LSTM
====

.. image:: /Stage_2/toplogyStructure/LSTM.png

.. code-block:: bash
   
   def forward(x, h_prev, C_prev):
      assert x.shape == (X_size, 1)
      assert h_prev.shape == (H_size, 1)
      assert C_prev.shape == (H_size, 1)

      z = np.row_stack((h_prev, x))
      f = sigmoid(np.dot(W_f, z) + b_f)
      i = sigmoid(np.dot(W_i, z) + b_i)
      C_bar = tanh(np.dot(W_C, z) + b_C)

      C = f * C_prev + i * C_bar
      o = sigmoid(np.dot(W_o, z) + b_o)
      h = o * tanh(C)

      y = np.dot(W_y, h) + b_y
      p = np.exp(y) / np.sum(np.exp(y))

      return z, f, i, C_bar, C, o, h, y, p
ESN
====

储备池，大小的选择，要与谱半径相关的。输入权值越小，而内部矩阵的谱半径越接近1，网络短期记忆时间越长。

LightRNN
========

不是用一个向量来表达一个词，而是用两个向量表达一个词，一个行向量，一个列向量，不同的词之间共享行或列向量。我们用一个二维的表格来表达整个词表，假设这个二维的表格有一千行一千列，这个表格可以表达一百万个词；这个表格的每一行有一个行向量，每一列有一个列向量，这样整个二维表格只需要两千个向量。如果一个词（January）在第一行第一列的话，它就由行向量X1和列向量Y1来联合表达。考虑一个有一百万个词的词表，原来需要一百万个嵌入向量，通过这样一个二维或者是两个component的表格词嵌入，现在我们只需要一千个行向量和一千个列向量来进行表达，这样大大降低词嵌入向量模型的大小。

结果表明我们提出的LightRNN算法极大的减小了模型的尺寸，可以把原来语言模型的大小从4G降到40M左右，当这个模型只有40兆的时候，很容易使得我们在移动设备或者是GPU上使用。我们的方法使得深度模型在各种能耗比较低或者内存比较小的设备上的使用成为了可能。并且我们还发现，通过这样一种共享的二维词表的嵌入，我们得到的循环神经网络模型的精度并没有受到很大的影响，实际上LightRNN的精度反而略微有上升，和前面的卷积神经网络压缩的结果比较类似。
具有相同特征值的连接矩阵的储备池，具有几乎相同的状态空间复杂。

并且也用泄漏积分神经元，以及小波函数展开当做神经元。
储备池性能的优化。

当前的进展
http://or.nsfc.gov.cn/bitstream/00001903-5/88718/1/1000002285062.pdf

而储备池的memory capacity. MC=N-(1-r^2N) 也就2^MC个。http://xueshu.baidu.com/s?wd=paperuri:(7d54c3ac9eacc58ee0079115894c6038)&filter=sc_long_sign&sc_ks_para=q%3DMinimum+complexity+echo+state+network.&tn=SE_baiduxueshu_c1gjeupa&ie=utf-8&sc_us=5816960841071681866

Echo 意义，当前状态只由过去的状态来决定，而输出是没有反馈到输入的。

另外现在直接端到端的直接语音到文本的转换，采用双向LSTM http://www.jmlr.org/proceedings/papers/v32/graves14.pdf。
还用利用组合神经网络的，一种是直接从初始数据直接使用神经网络来学习，另一种是采用以前知识进行一定的抽取再来进行神经网络学习。

IndRNN
======

https://github.com/gwli/indrnn-pytorch/blob/master/indrnn.py

现在只是把 U*h(t-1)的形式，变成了点乘，这样解决了累积消失的问题，相当于实现一个RNN的resnet,而传统的resNet对于CNN很有效，但是RNN不是很效的。IndRNN很好这个问题，步数可以达到5000步之多。
因为没有了矩阵乘累加，就是每一层的每一个神经元与其它都是独立的，它们之间的连接可以通用stacking 多层来解决。相当于ResNet的 X+wh,只与每个神经网自身的过去有关。
