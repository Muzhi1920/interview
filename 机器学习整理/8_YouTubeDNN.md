# YouTubeDNN

## 关键整理

- 1.召回TopN：生成倒排，近似搜索；
- 2.新视频：用该视频最近生成日志的时间example age；初始化训练时为0；就是说越短越可能为正样本（训练的trick）
- 3.测试集作为最近一次观看行为
- 4.优化目标非CTR，播放率而是曝光后预期播放时间
- 5.目标函数的设定应该是一个算法模型的根本性问题
- 6.对某些特征开方x，x，x^2处理后输入在线上serving中使用e(Wx+b)做预测可以直接得到expected watch time的近似。
- 7.Weighted LR的特点是，正样本权重w的加入会让正样本发生的几率变成原来的w倍，也就是说样本i的Odds变成了下面的式子：
![avatar](https://www.zhihu.com/equation?tex=Odds%28i%29%3D%5Cfrac%7Bw_ip%7D%7B1-w_ip%7D)
由于在视频推荐场景中，用户打开一个视频的概率p往往是一个很小的值，因此上式可以继续简化：![avatar](https://www.zhihu.com/equation?tex=Odds%28i%29%3D%5Cfrac%7Bw_ip%7D%7B1-w_ip%7D%5Capprox%7Bw_ip%7D%3DT_ip%3DE%28T_i%29)
因此，Model Serving过程中[公式] 计算的正是观看时长的期望。

## 模型架构
### 候选集生成
![avatar](https://pic3.zhimg.com/80/v2-0e0f8479c5260e4c5096f730dd6ea6ba_hd.jpg)
### 精排
![avatar](https://pic4.zhimg.com/v2-4dd8f94f0cf5becd7dd5ff4c0fd9393f_1200x500.jpg)