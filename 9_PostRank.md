# MMR(PostRank控制多样性)

![avatar](https://img-blog.csdn.net/20180725101556342?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1pKUk4xMDI3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

lambad 越大，准确性越大；
lambda 越小，多样性越好。


# 序列评估与生成框架

## 目标函数
总收益函数：![avatar](img/e1.png)

模型fθ拟合：![avatar](img/e2.png)

优化函数1：![avatar](img/e3.png)

最终优化函数2：![avatar](img/e4.png)

两层Transformer，4个head；最后得到某个位置item的概率。最后计算总体收益。

