# neuman背景训练
> 本文分析和阅读train.py的train_background
> 疑问：为什么能只保留背景？

### 一. 相关代码
- 创建模型coarse_net, fine_net，在models/vanilla.py
- BackgroundRayDataset负责背景训练的光线计算,在datasets/background_rays.py
- NeRFTrainer经典的nerf模型训练，在trainers/vanilla_nerf_trainer.py

### 二.创建模型
就是经典的nerf模型
- Embedder：位置编码，默认是位置编码，可选rotate旋转编码。
### 三. BackgroundRayDataset
读取数据，光线采样
### 四.  NeRFTrainer，基类BaseTrainer
```
- train
    - train_epoch
        - train_batch
```