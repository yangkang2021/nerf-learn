# neuman人体训练
> 本文分析和阅读train.py的train_human
> 疑问：为什么能剔除背景？

### 一. 相关代码
- 创建模型net = human_nerf.HumanNeRF，在models、human_nerf.py
- HumanRayDataset负责背景训练的光线计算,在datasets/human_rays.py
- HumanNeRFTrainer负责人体nerf模型训练，在trainers/human_nerf_trainer.py

### 二.创建模型

### 三. HumanRayDataset
读取数据，光线采样
### 四.  HumanNeRFTrainer，基类BaseTrainer
```
- train
    - train_epoch
        - train_batch
```