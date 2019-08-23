#  Tensorflow
- 首先通过将程序分为两个独立的部分，包括计算图的定义及其执行。
- 图定义和执行的分开设计让 TensorFlow 能够多平台工作以及并行执行。
- 计算图：是包含节点和边的网络。定义所有要使用的数据，也就是张量（tensor）对象（常量、变量和占位符），同时定义要执行的所有计算，即运算操作对象（Operation Object，简称 OP）。
- 每个节点可以有零个或多个输入，但只有一个输出。网络中的节点表示对象（张量和运算操作），边表示运算操作之间流动的张量。

---
## 数据类型：常量、变量、占位符
### 常量
常量是其值不能改变的张量；常量存储在计算图的定义中
```python
tf.constant(4);
tf.constant([4,3,2])
tf.zeros([M,N],tf.dtype)
tf.zeros([2,3],tf.int32)
tf.ones([M,N],tf,dtype)
range_t = tf.linspace(2.0,5.0,4)  #等差数列，差值=(5-2)/(4-1)
range_t = tf.range(10) #同python range
tf.random_normal([shape],mean=2.0,stddev=4,seed=12)
```

### 变量
在神经网络中的权重声明为变量来实现，变量在使用前需要被显示初始化。另外需要注意的是，每次加载图时都会加载相关变量。
```python
#常量初始变量
weights=tf.Variable(tf.random_normal([m,n],stddev=2))
bias=tf.Variable(tf.zeros[100],name='bias')
#变量初始变量
weights_2=tf.Variable(weights.initialized_value,name='w2')
intial_op=tf.global_variables_initializer()#全部变量初始化，部分变量初始化

```

### 占位符
和 feed_dict 一起使用于将训练数据输入TensorFlow图中。在session运行计算图时，为占位符赋值。占位符不包含任何数据，只是数据的入口；
```python
x=tf.placeholder(dtype,shape=None,name=None)
y=2*x
data=tf.random_uniform([4,5],10);
with tf.Session() as sess:
    x_data=sess.run(data)
    print(sess.run(y,feed_dict={x:x_data}))
```

## 矩阵运算
```python
#创建矩阵
W=tf.Variable(tf.random_uniform([m,n],10))
#矩阵相乘
tf.matmul(W,X).eval #取值
tf.add(w1,w2)
```

## Tensorflow的重要API
### 损失函数与优化器
```python
#平方损失
loss=tf.reduce_mean(tf.square(Y-Y_hat,name='loss'))
#交叉熵损失
entropy=tf.nn.softmax_cross_entropy_with_logits(Y_hat,Y)
loss=tf.reduce_mean(entropy)
#正则项系数
lambda=tf.constant(0.2)
#L1正则
l1_reg=lambda*tf.reduce_sum(tf.abs(w1))
loss+=l1_reg
#L2正则
l2_reg=lambda*tf.nn.l2_loss(W1)
loss+=l2_reg
# focal loss
preds = array_ops.where(math_ops.equal(labels, 1), predictions, 1. - predictions)
losses = -alpha * (1. - preds) ** gamma * math_ops.log(preds + epsilon)


tf.summary_scalar('loss')

train_step=optimizer=tf.train.AdamOptimizer.minimize(loss)
with tf.Session() as sess:
    sess.run(train_step,feed_dict={x:x_data,y:y_data})

```

### 激活函数
```python
tf.sigmoid(x)  #梯度消失严重
tf.tanh(x)  #会出现梯度消失
tf.nn.relu(x)  #神经元稀疏失活，leakrelu
tf.nn.softmax(x)    #多分类中表示一个类的概率
```


