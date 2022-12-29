# neuman环境搭建
```
C:\Program Files\OpenSSL-Win64\bin 放到path
启动anaconda prompt，不是powershell

#-----------------创建虚拟环境python=3.9-------------------
conda remove -n neuman_env --all
conda create -n neuman_env python=3.9
conda activate neuman_env
conda info --envs

#-----------------安装pytorch -------------------
#https://pytorch.org/get-started/previous-versions/
conda install numpy
conda install pytorch==1.9.1 torchvision==0.10.1 torchaudio==0.9.1 cudatoolkit=10.2 -c pytorch
# 测试代码 python: import torch ; print(torch.__version__,torch.cuda.is_available())

#-----------------安装pytorch3d-------------------
conda install -c fvcore -c iopath -c conda-forge fvcore iopath

# 无法安装
# conda install -c bottler nvidiacub

# 无法安装，用基于源码的
conda install pytorch3d -c pytorch3d

#https://github.com/facebookresearch/pytorch3d/blob/main/INSTALL.md#3-install-from-a-local-clone
set FORCE_CUDA=1
set DISTUTILS_USE_SDK=1
set TORCH_CUDA_ARCH_LIST=6.0;6.1
set PYTORCH3D_NO_NINJA=1
"C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\VC\Auxiliary\Build\vcvars64.bat"

pip install "git+https://github.com/facebookresearch/pytorch3d.git@stable"
# 测试代码 python: import pytorch3d; print(pytorch3d.__version__)

#-----------------安装其他依赖-------------------
pip install opencv-python joblib open3d imageio tensorboardX chumpy lpips scikit-image ipython matplotlib;

conda install -c conda-forge igl
# 测试代码 python: import igl; print(igl.__version__)



conda remove -n neuman_env
pip install -r requirements.txt 
```