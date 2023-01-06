# train.py代码框架
>train.py主要用来训练背景和人体。

### 一. 训练命令
先训练背景模型，背景模型会被加载到human模型。
```
训练背景命令
python train.py --scene_dir ./data/bike/ --name=bike_background_yk --train_mode=bkg
训练人体命令
python train.py --scene_dir ./data/bike  --name=bike_human_yk --load_background=bike_background_yk --train_mode=smpl_and_offset
```

### 二. train.py主要做三件事
1. opt解析
2. train_background(opt)：主要做4件事情：
    1. coarse_net, fine_net = vanilla.build_nerf(opt)：创建经典的nerf网络
	2. neuman_helper.NeuManReader.read_scene读取数据
	3. background_rays.BackgroundRayDataset创建训练集和验证集
	4. vanilla_nerf_trainer.NeRFTrainer训练数据
3. train_human(opt)：主要做4件事情：
	1. neuman_helper.NeuManReader.read_scene读取数据
	2. net = human_nerf.HumanNeRF创建模型
	3. human_rays.HumanRayDataset创建训练集和验证集
	4. human_nerf_trainer.HumanNeRFTrainer训练
### 三. 训练参数
1. train_mode：bkg，smpl_only，smpl_and_offset
2. load_weights/resume：接着之前的训练
3. 其他参数待续