

### 介绍
深度相机有3维点云信息，可利用深度相机改造成一个伪2d激光雷达

详情请点击
- [depthimage_to_laserscan](http://wiki.ros.org/depthimage_to_laserscan)

- [深度图转激光原理](https://blog.csdn.net/u014587147/article/details/77985508)
### 深度相机Intel RealSense D435i在ROS下的使用

1. 克隆深度相机驱动包至工作空间
```shell
git clone https://github.com/IntelRealSense/realsense-ros.git
```
2. 启动深度相机
```shell
roslaunch realsense2_camera rs_camera.launch
```
3. 查看深度图像
```shell
rqt_image_view
```
下图是我截取深度相机的图像
![5ju1G.png](https://i.w3tt.com/2021/11/01/5ju1G.png)

### depthimage_to_laserscan
1. 克隆功能包至工作空间
```shell
git clone https://github.com/ros-perception/depthimage_to_laserscan.git
```
将`launchfile_sample.launch`里面的深度相机topic改成你的，我这里是`/camera/depth/image_rect_raw`
```html title='depthimage_to_laserscan/launch/launchfile_sample.launch'
<launch>

  <!-- Depth Image to Laser Scan Node -->
  <node name="depthimage_to_laserscan" pkg="depthimage_to_laserscan" type="depthimage_to_laserscan" args="">

    <remap from="image"       to="/camera/depth/image_rect_raw"/> <!-- change here for your camera depth topic name. Default: "/camera/depth/image_raw" -->

    <remap from="camera_info" to="/camera/color/camera_info"/> 
    <!-- the remap for camera_info by default uses the same path as the one given to the image topic. Default: "<path_to_image_topic>/camera_info>" -->

    <!-- PARAMETERS: -->
    <!-- These are the default values. --> 
    <param name="scan_height"     type="int"    value="1"/> <!-- default: 1 pixel. Number of pixel rows used to generate laser scan. -->
    <param name="scan_time"       type="double" value="0.033"/> <!-- default:0.033, 30 FPS . Time between scans. -->
    <param name="range_min"       type="double" value="0.45"/> <!--default:0.45m. Ranges less than this are considered -Inf. -->
    <param name="range_max"       type="double" value="10.0"/> <!--default: 10m. Ranges less than this are considered +Inf. -->
    <param name="output_frame_id" type="str"    value="camera_depth_frame"/> <!--default: camera_depth_frame. Frame id of the laser scan. -->
	
  </node>
	
</launch>
```
2. 启动深度信息转换成`/scan`信息
```shell
roslaunch depthimage_to_laserscan launchfile_sample.launch
```
3. 启动rviz查看`/LaserScan`信息
![5jIH1.png](https://i.w3tt.com/2021/11/01/5jIH1.png)

### 参考致谢
- [RGBD相机Intel RealSense D435i的使用](https://blog.csdn.net/YuhsiHu/article/details/113725802)
- [ROS'wiki](http://wiki.ros.org/depthimage_to_laserscan)
- [深度图转激光原理](https://blog.csdn.net/u014587147/article/details/77985508)

