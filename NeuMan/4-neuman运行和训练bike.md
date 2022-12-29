# neuman运行和训练bike
>输出到demo目录下

### 一. 运行
```
C:\Program Files\OpenSSL-Win64\bin 放到path
启动anaconda prompt，不是powershell
conda activate neuman_env
```
---
### 渲染人的经典动作，白背景
```
python render_360.py --scene_dir ./data/bike --weights_path ./out/bike_human/checkpoint.pth.tar --mode canonical_360 --use_cuda true
```

### 渲染人的姿态，在训练集里面的动作，白背景
```
python render_360.py --scene_dir ./data/bike --weights_path ./out/bike_human/checkpoint.pth.tar --mode posed_360 --use_cuda true
```
---

```
python render_test_views.py --scene_dir ./data/bike --weights_path ./out/bike_human/checkpoint.pth.tar --use_cuda true
```

### 带背景的，带新动作的渲染
```
python render_reposing.py --scene_dir ./data/bike --weights_path ./out/bike_human/checkpoint.pth.tar --motion_name=jumpandroll --use_cuda true
```

### 把parkinglot seattle citron三个场景的人渲染到seattle场景
```
python render_gathering.py --actors parkinglot seattle citron --scene_dir ./data/seattle --weights_path ./out/seattle_human/checkpoint.pth.tar --use_cuda true
```

### 二. 训练
训练自行车场景
python train.py --scene_dir ./data/bike/ --name=bike_background_yangkang --train_mode=bkg --use_cuda true

训练自行车场景的人
python train.py --scene_dir ./data/bike  --name=bike_human_yangkang --load_background=bike_background_yangkang --train_mode=smpl_and_offset --use_cuda true