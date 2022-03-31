---
title: Hierarchical Neural Story Generation
date: 2022-03-29 15:26:33
tags:
 - Python
 - NLP
 - NLG
categories: 文本生成
cover: https://od.alonesoul.club/api?path=/Blog/hexo/paper_review.png&raw=true
---

### 概括
论文首先指出两点：

1. 生成提示可以使故事生成具有基础，这样生成的故事就不会轻易偏离主题，这在`Seq2Seq`模型中很常见。
2. 标准的`Seq2Seq`模型往往会忽略提示，因此他们提出了解决此问题的方案。

### CNN Seq2Seq的优势
本文采用`CNN Seq2Seq`模型，相比于`RNN Seq2Seq`有两个优点：

1. 运行速度快（`CNN`可以并行计算，但`RNN`需要等上一个时间步计算完毕才能进行下一步的运算）。
2. 更容易捕获单词之间不同长度的依赖关系（在一组堆叠的卷积层中，底层捕获更紧密的依赖关系，而顶层提取单词间更长的依赖关系）。
![依赖关系](https://od.alonesoul.club/api?path=/Blog/20220330/%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB.gif&raw=true)

### Conv Seq2Seq
下图展示了`Conv Seq2Seq`的结构。该模型顶部和底部分别为`Encoder（编码器）`和`Decoder（解码器）`，中间是基本的`Attention（注意力）`。
![Conv Seq2Seq](https://od.alonesoul.club/api?path=/Blog/20220330/Conv_Seq2Seq.jpg&raw=true)

`Encoder`和`Decoder`都有堆叠的卷积层组成。为了简单，该图仅显示了一层。
![Conv Seq2Seq](https://od.alonesoul.club/api?path=/Blog/20220330/Conv_Seq2Seq2.jpg&raw=true)

`Encoder`从原始序列中提取特征，`Decoder`学会估计将`Encoder`的隐藏状态和之前生成的单词映射到下一个单词的函数，`Attention`会告诉`Encoder`需要关注`Encoder`的哪些隐藏状态。
![Conv Seq2Seq](https://od.alonesoul.club/api?path=/Blog/20220330/Conv_Seq2Seq3.jpg&raw=true)

###  Gated Linear Unit(GLU) 
下图展示了GLU的工作原理，`W`和`V`是两个卷积核，通过滑动嵌入向量`E`，卷积层的输出被分为`A`和`B`两部分。
![GLU](https://od.alonesoul.club/api?path=/Blog/20220330/conv.jpg&raw=true)

`A`通过一个`sigmoid`函数，来充当门，确定`B`的哪些部分与当前语境相关，`A`中某条目的价值越大，`B`中对应的条目就越重要，图中蓝色圈中的符号表示点积，这种门控机制使模型能够选择输入特征的有效部分来预测下一个单词。
![GLU](https://od.alonesoul.club/api?path=/Blog/20220330/conv2.jpg&raw=true)

### Attention
根据论文`《Attention Is All You Need》`，Attention可以正式被描述为`Queries`，`Keys`，`Values`的函数。输出的为`values`的加权和，权重`w`是`queries`和`keys`点积运算的结果。
$$
f(queries, keys, values)= \sum w * values
$$

下图描绘了一个基本的注意力机制，`Query`是`Decoder`的一种隐藏状态，而`Keys`和`Values`都是`Encoder`的隐藏状态，图中黄色的单元格表示对于特定`Dncoder`的隐藏状态，`Encoder`输入的该文本比其它文本更受关注。
![Attenion](https://od.alonesoul.club/api?path=/Blog/20220330/Attention.jpg&raw=true)

#### Self-Attention
对于`Self-Attention（自注意力）`，`Quers`，`Keys`，`Values`来自同一来源，但应用了不同的线性操作。在本文中`Self-Attention`被用于连接`Decoder`中的卷积层。图中红色圆圈中三部分都是来自于`Decoder`的隐藏状态。`Queries`和`Keys`通过几个全连接层、下采样层和门控线性单元，而对`Values`的操作较少，本文后边会解释问什么会有下采样层。图中`Queries`和`Values`经过上述操作后，两者进行`点积运算`和`softmax`以形成`Values`的权重分布，加权后`Values`将作为下一个卷积层的输入。
![Self Attenton](https://od.alonesoul.club/api?path=/Blog/20220330/Self-Attention.jpg&raw=true)

#### Multi-head Self-Attention
该文章进一步提出使用多头机制来丰富`Self-Attention`的信息，多头`Self-Attention`的本质就是几个`Self-Attention`组成的方法。它的输出是这些`Self-Attention`的串联。文章中提到不同的头会学习到不同的单词间的联系。

例如，下图显示了来自两个不同头的注意力结果。图中左边为输入，右边为输出。中间的连线显示了输出所关注的是输入哪一部分。从图中可以看出，左边的头更关注附近的词，而右边的头更多的关注距离较远的词。
![Multi-head](https://od.alonesoul.club/api?path=/Blog/20220330/Multi-head.jpg&raw=true)
为了使这些头关注的内容多样化，文中提出了`Multi-scale Mechanism（多尺度）`，该机制利用前面提出的下采样层。第一个头可以看到完整的输入，第二个头看到的是各隔一个时间步的输入，以此类推，强制每个头部专注于上下文不同的部分。
![Multi-head](https://od.alonesoul.club/api?path=/Blog/20220330/Multi-head2.jpg&raw=true)

为了捕获文本之间的时间状态，本文采用了一种称为`Position Embedding（位置嵌入）`的技术，它的工作原理与常规的`Word Embedding（词嵌入）`相似，但它不是映射单词，而是将单词的绝对位置映射到密集向量。位置嵌入的输出叠加到词嵌入上，有了这些额外的信息，模型就能够知道正在处理是上下文的哪一部分。
![Position Embedding](https://od.alonesoul.club/api?path=/Blog/20220330/Embedding.jpg&raw=true)

正如之前所提到的，`Seq2Seq`模型在生成提示和故事时，它容易忽略提示而只关注故事的生成。文章中称导致这种情况是因为与故事之间的依赖关系相比，提示和故事之间的依赖关系更难建模。为了解决这一问题，文中提出了一种增加预训练模型的方法。
他们将预训练好的`CNN Seq2Seq`模型和当前训练的模型和隐藏状态连接到一起，并学习一个选择两个隐藏状态中重要因素的门。它可以表述如下：
$$
g_{t} = \sigma(W[h_{t}^{Training}; h_{t}^{Pretrained}] + b)
$$

$$
h_{t} = g_{t} \circ [h_{t}^{Training}; h_{t}^{Pretrained}]
$$

$g_t$为输出门，$h_t$表示最终联合输出。本质上$g_t$是每个隐藏状态的线性投影，通过`sigmoid`函数确定$h_{t}^{Training}$和$h_{t}^{Pretrained}$的哪些部分与当前上下文相关。文中认为，预训练模型的融合就像`Skin Connection`，它鼓励模型的另一部分专注于失败的部分，例如，对故事和提示之间的依赖关系进行建模。
![Skin Connection](https://od.alonesoul.club/api?path=/Blog/20220330/Skin_Connection.jpg&raw=true)
