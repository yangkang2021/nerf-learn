# neuman数据准备

1. 下载人体模型文件SMPL到：data/smplx/smpl_uv.obj
    - SMPL：A Skinned Multi-Person Linear Model。
    - SMPL 是一种逼真的人体 3D 模型, 可以用代码控制。
    - 下载地址：https://download.is.tue.mpg.de/download.php?domain=smpl&sfile=smpl_uv_20200910.zip
    - ![](.images/b6ffde04.png)
2. 下载从图片估计人体模型的模型Smplify到：data/smplx/smpl/SMPL_NEUTRAL.pkl
    - 下载地址：https://download.is.tue.mpg.de/download.php?domain=smplify&resume=1&sfile=mpips_smplify_public_v2.zip
3. 下载数据集预训练的模型
    - 数据下载到data下，共六个数据集： 
        - bike
        - citron
        - jogging
        - lab
        - parkinglot
        - seattle
    - 下载预训练的模型到out目录，共8个：
        - bike_human
        - citron_human
        - jogging_human
        - lab_human
        - parkinglot_human
        - seattle_human
        - seattle_no_condition
        - seattle_no_offset
    - 参考文件setup_data_and_models.sh
    ```
        echo "Downloading data"
        wget -P data/ https://docs-assets.developer.apple.com/ml-research/datasets/neuman/dataset.zip
        echo "Downloding models"
        wget -P out/ https://docs-assets.developer.apple.com/ml-research/datasets/neuman/pretrained.zip
        echo "Extracting data"
        unzip -q data/dataset.zip -d data/
        echo "Extracting models"
        unzip -q out/pretrained.zip -d out/
        echo "Cleaning up"
        mv data/dataset/* data/
        mv out/pretrained/* out/
        rm data/dataset.zip 
        rm out/pretrained.zip
        rm -r data/dataset 
        rm -r data/pretrained
      ```
4. 下载AMASS到：data/sfu
    - AMASS: Archive of Motion Capture As Surface Shapes
    - 下载地址：https://download.is.tue.mpg.de/download.php?domain=amass&resume=1&sfile=amass_per_dataset/smplh/gender_specific/mosh_results/SFU.tar.bz2