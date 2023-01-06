# 6-neuman训练数据加载
> 文本主要阅读NeuManReader，在data_io/neuman_helper.py

### 一. 相关代码
- ColmapAsciiReader读取场景，在data_io/colmap_helper.py
- ImageFileScene/RigCameraScene等，在scenes/scene.py
- RGBPinholeCapture等，在cameras/captures.py
- CapturedContent等，在cameras/contents.py
- SMPL,在model/smpl.py

### 二. 代码流程
- NeuManReader.read_scene
    - read_scene(4步)
        - read_captures
            - colmap_helper.ColmapAsciiReader.read_scene
                - read_captures
                    - read_cameras
                    - read_images_meta
                    - RGBPinholeCapture/ResizedRGBPinholeCapture
                - read_point_cloud
                - scene_module.ImageFileScene
            -
        - update_near_far
        - read_smpls //读取了四个文件的信息
            - body_model = SMPL('data/smplx/smpl/SMPL_NEUTRAL.pkl',...)
            - joblib.load('smpl_output_romp.pkl')
            - np.load(os.path.join(scene_dir, 'alignments.npy')
            - extract_smpl_at_frame
            - da_smpl
            - T_t2pose
            - T_t2da
        - uvs, faces = utils.read_obj('data/smplx/smpl_uv.obj',...)
        - update_near_far

### 三. 数据结构
RigCameraScene包含NeuManCapture而不是Capture包，Capture含了CapturedContent

Scene结果存储：
- RigCameraScene
        - num_views
        - num_cams
        - view_id_to_index
        - cam_id_to_index
    - ImageFileScene
        - image_path_to_index
        - fname_to_index_dict
        - BaseScene
            - captures //NeuManCapture
            - point_cloud //还有点云数据
        - smpls，verts，static_vert，Ts，uvs，faces //SMPL模型数据

PinholeCapture的定义：image_path, cam, camera_pose
- RigPinholeCapture
- ResizedPinholeCapture
- DepthPinholeCapture
- RGBPinholeCapture
- 及其组合
- NeuManCapture：RigRGBDPinholeCapture
    - captured_mask
    - captured_mono_depth
    - keypoints
    - densepose

CapturedContent的定义
- CapturedImage
- ResizedCapturedImage
- CapturedDepth
- ResizedCapturedDepth
---
SMPL模型定义

### 四. 参考
-[【PyTorch】torch.utils.data.Dataset 介绍与实战](https://blog.csdn.net/weixin_44211968/article/details/123744513)