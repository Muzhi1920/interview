# Transformer
![avatar](https://img-blog.csdnimg.cn/20181218173013772)
![avatar](https://upload-images.jianshu.io/upload_images/12877808-2d01f4df4f996b8f.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/640/format/webp)
---
## Encoder
![avatar](https://img-blog.csdnimg.cn/20181218173013893)
Transformer的encoder中，数据首先会经过一个叫做‘self-attention’的模块得到一个加权之后的特征向量$Z$，这个$Z$便是论文公式1中的 $\text{Attention}(Q,K,V)$
$$\text{Attention}(Q,K,V)=\text{softmax}(\frac{QK^T}{\sqrt{d_k}})V \tag1$$

得到$Z$之后，它会被送到encoder的下一个模块，即Feed Forward Neural Network。这个全连接有两层，第一层的激活函数是ReLU，第二层是一个线性激活函数，可以表示为：
$$\text{FFN}(Z) = max(0, ZW_1 +b_1)W_2 + b_2 \tag2$$

### 从self-Attention到MultiHead-Attention

在self-attention中，每个单词有3个不同的向量，它们分别是Query向量（ Q ），Key向量（ K ）和Value向量（ V ），长度均是64。它们是通过3个不同的权值矩阵由嵌入向量 X 乘以三个不同的权值矩阵$W^Q$，$W^K$,$W^V$ 得到，其中三个矩阵的尺寸也是相同的。均是$512\times 64$.
- 图解![avatar](https://img-blog.csdnimg.cn/20181218173013842)

1. 根据嵌入向量得到 q ， k ， v 三个向量;
2. 为每个向量计算一个score：$\text{score} = q \cdot k$;
3. 为了梯度的稳定，Transformer使用了score归一化，即除以$\sqrt{d_k}$;
4. 对score施以softmax激活函数，归一化权重
5. softmax点乘Value值 v，得到加权的每个输入向量的评分 v
6. 对每个词的emb相加之后得到最终的输出结果$z ： z=\sum v$.
- 图解![avatar](https://img-blog.csdnimg.cn/20181218173013859)

以上为self-Attention，MultiHead-Attention如下
![avatar](https://img-blog.csdnimg.cn/20181218173013927)
将各路self-Attention结果拼接得到MultiHead-Attention


## Position encoding
至此无论句子的结构怎么打乱，Transformer都会得到类似的结果。换句话说，Transformer只是一个功能更强大的词袋模型而已。引入位置信息的方式是对位置进行编码论文给出的编码公式如下：
$$PE(pos, 2i) = sin(\frac{pos}{10000^{\frac{2i}{d_{model}}}}) \tag3$$
$$PE(pos, 2i+1) = cos(\frac{pos}{10000^{\frac{2i}{d_{model}}}}) \tag4
$$
在上式中， pos 表示单词的位置， i 表示单词的维度。在NLP任务重，除了单词的绝对位中，单词的相对位置也非常重要。根据公式$sin(\alpha+\beta) = sin \alpha cos \beta + cos \alpha sin\beta$以及$cos(\alpha + \beta) = cos \alpha cos \beta - sin \alpha sin\beta$这表明位置 k+p 的位置向量可以表示为位置 k 的特征向量的线性变化，这为模型捕捉单词之间的相对位置关系提供了非常大的便利。

## 总结：
### 优点
1. 虽然Transformer最终也没有逃脱传统学习的套路，Transformer也只是一个全连接（或者是一维卷积）加Attention的结合体。抛弃了在NLP中最根本的RNN或者CNN并且取得了非常不错的效果;
2. Transformer的设计最大的带来性能提升的关键是self_attention将任意两个单词的求取权重，解决文本处理中的长期依赖问题;
3. Transformer可以应用更广;
4. 算法可并行，支持GPU加速。

### 缺点
1. 位置信息不够重复；











