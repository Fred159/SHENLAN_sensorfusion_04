# SHENLAN_sensorfusion_03
SEHNLANXUEYUAN sensor fusion course homework-chapter 3 mapping and matching

### 1 介绍
本次作业的目标为建立地图，并根据构建的地图通过匹配的方式进行定位。
本次利用提供的pkg构建了地图，但由于电脑内存有限，只利用kittidata的前200秒的数据构建了地图。然后在利用地图进行匹配定位的过程中，利用了scancontext的方式进行了匹配。
系统配置为ubuntu 16.04， i7-8th， 12gb内存。

### 2 步骤

- 1.在提供的docker环境中，利用提供源码和kitti数据集进行建立地图。由于内存的问题，电脑在kitti数据播放到250以上的时候会出现内存不足导致的代码终止问题。所以在200s左右的时候，依次执行，优化，存图，存scancontext数据。

![建立的地图](https://github.com/Fred159/SHENLAN_sensorfusion_03/blob/main/figures/mapped_map.png)


- 2.在2.1的步骤中，正常的话，会在src的slamdata路径下的map中会产生两个pcd文件。那么在确认有这个文件的情况下，通过roslaunch运行matching步骤。matching的方式这里利用了scancontext的方式。在本github repo的matching_flow.cpp的第144行中，添加下面这一行代码。
       
```
    matching_ptr_->SetScanContextPose(current_cloud_data_);
```

- 3.SetGNSSPose的方法也尝试过，但是不知道为什么总是在旋转。读取了kittidata的第一帧的long/lat数据数据过后，通过SetGNSSPose的方式设定了matching_ptr_，但是没有成功匹配。暂时还不知道原因。
```
    // SetGNSSPose approach
    // Eigen::Matrix4f init_pose= Eigen::Matrix4f::Identity();

    // init_pose(0,3) = 8.39036610005;
    // init_pose(1,3) = 48.9825452359;
    // init_pose(2,3) = 116.382141113;

    // matching_ptr_->SetGNSSPose(init_pose);
```
对应的图片如下。
![ScanContextBasedMatching](https://github.com/Fred159/SHENLAN_sensorfusion_03/blob/main/figures/ScanContextBasedMatching1.png)
![ScanContextBasedMatching](https://github.com/Fred159/SHENLAN_sensorfusion_03/blob/main/figures/ScanContextBasedMatching2.png)
![ScanContextBasedMatching](https://github.com/Fred159/SHENLAN_sensorfusion_03/blob/main/figures/ScanContextBasedMatching3.png)
![ScanContextBasedMatching](https://github.com/Fred159/SHENLAN_sensorfusion_03/blob/main/figures/ScanContextBasedMatching4.png)
![ScanContextBasedMatching](https://github.com/Fred159/SHENLAN_sensorfusion_03/blob/main/figures/ScanContextBasedMatching5.png)
![ScanContextBasedMatching](https://github.com/Fred159/SHENLAN_sensorfusion_03/blob/main/figures/ScanContextBasedMatching6.png)
