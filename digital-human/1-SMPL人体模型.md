# SMPL人体模型
> smpl-ptyorch的代码https://github.com/CalciferZh/SMPL.git

### 一. 把模型内容输出看看(preprocess.py)
>smpl模型是一个具有14key的字典

- 代码
```
  src_path = 'models/basicModel_m_lbs_10_207_0_v1.0.0.pkl'
  with open(src_path, 'rb') as f:
    src_data = pickle.load(f, encoding="latin1")

    print('smpl model is:',type(src_data))
    for i,k in enumerate(src_data):
       v = src_data[k]
       print(str(i+1)+". SMPL["+k+"]", "=".ljust(25-len(k)),type(v),v.shape if hasattr(v,"shape") else v)
```
- 输出: smpl model is: <class 'dict'>
1. SMPL[J_regressor_prior] =        <class 'scipy.sparse.csc.csc_matrix'> (24, 6890)
2. SMPL[f] =                        <class 'numpy.ndarray'> (13776, 3)
3. SMPL[J_regressor] =              <class 'scipy.sparse.csc.csc_matrix'> (24, 6890)
4. SMPL[kintree_table] =            <class 'numpy.ndarray'> (2, 24)
5. SMPL[J] =                        <class 'numpy.ndarray'> (24, 3)
6. SMPL[weights_prior] =            <class 'numpy.ndarray'> (6890, 24)
7. SMPL[weights] =                  <class 'numpy.ndarray'> (6890, 24)
8. SMPL[vert_sym_idxs] =            <class 'numpy.ndarray'> (6890,)
9. SMPL[posedirs] =                 <class 'numpy.ndarray'> (6890, 3, 207)
10. SMPL[pose_training_info] =       <class 'dict'> {'expid': 'tj10smooth', 'J_regressor_prior_wt': 10.0, 'restpose_type': 't', 'bs_style': 'lbs', 'gender': 'male', 'bs_type': 'lrotmin'}
11. SMPL[bs_style] =                 <class 'str'> lbs
12. SMPL[v_template] =               <class 'numpy.ndarray'> (6890, 3)
13. SMPL[shapedirs] =                <class 'chumpy.ch.Ch'> (6890, 3, 10)
14. SMPL[bs_type] =                  <class 'str'> lrotmin
### 二. 模型的使用(smpl_np.py)
> 三个参数定义模型：beta，pose，trans
```
if __name__ == '__main__':
  smpl = SMPLModel('./model.pkl')
  np.random.seed(1024)
  pose = (np.random.rand(*smpl.pose_shape) - 0.5) * 0.4
  beta = (np.random.rand(*smpl.beta_shape) - 0.5) * 0.06
  trans = np.zeros(smpl.trans_shape)
  smpl.set_params(beta=beta, pose=pose, trans=trans)
  smpl.save_to_obj('./smpl_np.obj')
```
### 三. 模型输出到obj
> 就是把顶点verts和面faces写入文件， faces直接来自模型的f参数， verts需要计算。
```
class SMPLModel():
  ...
  def save_to_obj(self, path):
    with open(path, 'w') as fp:
      for v in self.verts:
        fp.write('v %f %f %f\n' % (v[0], v[1], v[2]))
      for f in self.faces + 1:
        fp.write('f %d %d %d\n' % (f[0], f[1], f[2]))
```
### 四. obj模型的顶点verts的计算
```
class SMPLModel():
  ...
  def update(self):
    """
    Called automatically when parameters are updated.

    """
    # how beta affect body shape
    v_shaped = self.shapedirs.dot(self.beta) + self.v_template
    # joints location
    self.J = self.J_regressor.dot(v_shaped)
    pose_cube = self.pose.reshape((-1, 1, 3))
    # rotation matrix for each joint
    self.R = self.rodrigues(pose_cube)
    I_cube = np.broadcast_to(
      np.expand_dims(np.eye(3), axis=0),
      (self.R.shape[0]-1, 3, 3)
    )
    lrotmin = (self.R[1:] - I_cube).ravel()
    # how pose affect body shape in zero pose
    v_posed = v_shaped + self.posedirs.dot(lrotmin)
    # world transformation of each joint
    G = np.empty((self.kintree_table.shape[1], 4, 4))
    G[0] = self.with_zeros(np.hstack((self.R[0], self.J[0, :].reshape([3, 1]))))
    for i in range(1, self.kintree_table.shape[1]):
      G[i] = G[self.parent[i]].dot(
        self.with_zeros(
          np.hstack(
            [self.R[i],((self.J[i, :]-self.J[self.parent[i],:]).reshape([3,1]))]
          )
        )
      )
    G = G - self.pack(
      np.matmul(
        G,
        np.hstack([self.J, np.zeros([24, 1])]).reshape([24, 4, 1])
        )
      )
    # transformation of each vertex
    T = np.tensordot(self.weights, G, axes=[[1], [0]])
    rest_shape_h = np.hstack((v_posed, np.ones([v_posed.shape[0], 1])))
    v = np.matmul(T, rest_shape_h.reshape([-1, 4, 1])).reshape([-1, 4])[:, :3]
    self.verts = v + self.trans.reshape([1, 3])
```