## centerpoint



## pvrcnn

cvpr2020

是结合了点和voxel的方法进行特征提取，3D voxel卷积高效，而point-based的方法感受野可变，因此结合了这两种检测方法的优点。（高效+可变感受野）

采用multi-scale的方法获得由voxel_based方法得到的高质量的proposals，然后再利用Point_based的方法获得精细的局部信息。

- 核心也就是如何将上述的两种方法有效的结合起来，这里作者的做法是：在每一个3D proposals内平均的采样一些Grid-point，然后再通过FPS最远点采样的方法得到该Grid_point周围的点，再通过结合去进一步refine最后的proposals

- voxel分支  正常的3d稀疏卷积

- point分支，先用fps在lidar场景中选kitti 2048  waymo 4096个点，这些点叫key points，然后经历vsa （voxel-to-keypoint scene encoding via voxel set abstraction ）这个模块将keypoints周围非空的voxel特征采集出来结合在一起

  ![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2pwZy9RMEZOVEIxWEhpY3dKaWN2VDhhRWljYkt2WE5NaWJZMHdpYTVNTGhDbnAzYTl6bVZTWDVyYmljeWdrSDBuNThGZzRtNlgyS3ZyNFNZVlZ1dXpicllqRlBGMVhTdy82NDA?x-oss-process=image/format,png)

- Extended VSA Module  融合三种feature（ pv，raw原始点云特征，bev）

- keypoint weighting module 一个模块，判断上述提到的keypoint是前景还是背景给一个权重

roi-grid pooling module 得到proposal之后，在每个proposal中均匀的采样grid points，再收集grid points球面体内的keypoints，融合keypoints 的feature 

