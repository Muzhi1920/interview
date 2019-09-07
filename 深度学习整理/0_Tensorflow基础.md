#  Tensorflow
- 首先通过将程序分为两个独立的部分，包括计算图的定义及其执行。
- 图定义和执行的分开设计让 TensorFlow 能够多平台工作以及并行执行。
- 计算图：是包含节点和边的网络。定义所有要使用的数据，也就是张量（tensor）对象（常量、变量和占位符），同时定义要执行的所有计算，即运算操作对象（Operation Object，简称 OP）。
- 每个节点可以有零个或多个输入，但只有一个输出。网络中的节点表示对象（张量和运算操作），边表示运算操作之间流动的张量。

---
## 一、数据类型：常量、变量、占位符
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

## 二、矩阵运算
```python
#创建矩阵
W=tf.Variable(tf.random_uniform([m,n],10))
#矩阵相乘
tf.matmul(W,X).eval #取值
tf.add(w1,w2)
```

## 三、Tensorflow的重要API
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


## 四、Tenforflow实现线性回归
```python

#使用numpy生成200个随机点
x_data=np.linspace(-0.5,0.5,200)[:,np.newaxis]
noise=np.random.normal(0,0.02,x_data.shape)
y_data=np.square(x_data)+noise
 
#定义两个placeholder存放输入数据
x=tf.placeholder(tf.float32,[None,1])
y=tf.placeholder(tf.float32,[None,1])
 
#定义神经网络中间层
Weights_L1=tf.Variable(tf.random_normal([1,10]))
biases_L1=tf.Variable(tf.zeros([1,10]))    #加入偏置项
Wx_plus_b_L1=tf.matmul(x,Weights_L1)+biases_L1
L1=tf.nn.tanh(Wx_plus_b_L1)   #加入激活函数
 
#定义神经网络输出层
Weights_L2=tf.Variable(tf.random_normal([10,1]))
biases_L2=tf.Variable(tf.zeros([1,1]))  #加入偏置项
Wx_plus_b_L2=tf.matmul(L1,Weights_L2)+biases_L2
prediction=tf.nn.tanh(Wx_plus_b_L2)   #加入激活函数
 
#定义损失函数（均方差函数）
loss=tf.reduce_mean(tf.square(y-prediction))
#定义反向传播算法（使用梯度下降算法训练）
train_step=tf.train.GradientDescentOptimizer(0.1).minimize(loss)
 
with tf.Session() as sess:
    #变量初始化
    sess.run(tf.global_variables_initializer())
    #训练2000次
    for i in range(2000):
        sess.run(train_step,feed_dict={x:x_data,y:y_data})
    #获得预测值
    prediction_value=sess.run(prediction,feed_dict={x:x_data})
```

## 五、estimator实现

- Transformer.py：完成Transformer模型构建
- Evaluator.py：该类集成了训练参数，和各子模型。搭建transformer+DNN
- main.py：训练入口


```python
def model_fn(features, labels, mode):
    params = {
        "batch_size": FLAGS.batch_size,
        "feature_dim": FLAGS.feature_dim,
        "length": FLAGS.length,
        "l2_reg": FLAGS.l2_reg,
        "learning_rate": FLAGS.learning_rate,
        "latent_layer": FLAGS.latent_layer,
        "cell_dim": FLAGS.cell_dim,
        "optimizer": FLAGS.optimizer,
        "dropout_keep_prob_p": FLAGS.dropout_keep_prob_p,
        "sequence_encoder": FLAGS.sequence_encoder
    }
    model = Evaluator(features,
                      labels,
                      mode,
                      params)
    return model.build_graph()
def main():
    run_config = tf.estimator.RunConfig(save_summary_steps=FLAGS.batch_size * 10,keep_checkpoint_max=3)
    classifier = tf.estimator.Estimator(
        model_fn=model_fn,
        model_dir=FLAGS.checkpoint_path,
        config=run_config
    )

    if FLAGS.mode == "train_and_eval":
        train_spec = tf.estimator.TrainSpec(
            input_fn=lambda: _input_fn(FLAGS.train_path, FLAGS.batch_size, epochs=FLAGS.epochs, prefetch=True))

        eval_spec = tf.estimator.EvalSpec(
            input_fn=lambda: _input_fn(FLAGS.test_path, FLAGS.batch_size, epochs=FLAGS.epochs, prefetch=False),
            throttle_secs=60
        )
        train, val = tf.estimator.train_and_evaluate(
            classifier,
            train_spec,
            eval_spec)
       
    elif FLAGS.mode == "eval":
        print("checkpoint_path {}".format(FLAGS.checkpoint_path))
        evaluate_result = classifier.evaluate(
            input_fn=lambda: _input_fn(FLAGS.test_path, FLAGS.batch_size, epochs=FLAGS.epochs, prefetch=False),
            steps=2,
            checkpoint_path=FLAGS.checkpoint_path,
        )
        for key in evaluate_result:
            print("   {}, was: {}".format(key, evaluate_result[key]))

```

