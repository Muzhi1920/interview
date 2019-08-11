
深度学习训练，batch_size*\length=8*\12=96.96个fid计算一次AUC，增大batch_size，多次shuffle。
计算AUC时当下多个样本二分类出现只有一种类别。主要是负样本的影响全0