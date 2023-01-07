# NeuMan系列文章
1. [neuman简介](./1-neuman简介.md)
1. [neuman环境搭建](./2-neuman环境搭建.md)
1. [neuman数据准备](./3-neuman数据准备.md)
1. [neuman运行demo](./4-neuman运行和训练bike.md)
1. [neuman自定义数据集之预处理](./5-neuman自定义数据集之预处理.md)
1. [neuman的train.py代码框架](./6-neuman的train.py代码框架.md)
1. [neuman训练数据加载](./7-neuman训练数据加载.md)
1. [neuman背景训练](./8-neuman背景训练.md)
1. [neuman人体训练](./9-neuman人体训练.md)
1. [neuman推理之渲染360度人体动作](./10-neuman推理之渲染360度人体动作.md)
1. [neuman推理之渲染测试视角](./11-neuman推理之渲染测试视角.md)
1. [neuman推理之渲染新的指定动作](./12-neuman推理之渲染新的指定动作.md)
1. [neuman推理之多人合并一起渲染](./13-neuman推理之多人合并一起渲染.md)


# Python相关知识
- [【PyTorch】torch.utils.data.Dataset 介绍与实战](https://blog.csdn.net/weixin_44211968/article/details/123744513)
- PyTorch的DataParallel
- PyTorch的train和eval：切换训练和评估(推断)的模式
- PyTorch遍历参数:
    - model.parameters()
    - model.named_parameters():keepvars?
    - model.state_dict()
- tqdm的用法
- 导出模型
    ```
        #导出模型看看
        torch.onnx.export(model, (torch.randn(1, 90)), "./nerf-model.onnx", verbose=True,training=torch.onnx.TrainingMode.EVAL);
        torch.onnx.export(model_fine, (torch.randn(1, 90)), "./nerf-model-fine.onnx", verbose=True, training=torch.onnx.TrainingMode.EVAL);
        print("+++++++torch.onnx.export finished+++++++")
    ```