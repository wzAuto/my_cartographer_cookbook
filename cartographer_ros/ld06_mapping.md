# ld06 mapping example

## 实现的功能
利用录制好的rosbag建图，将地图保存为pbstream格式，与rosbag同目录。  
使用全速建图，避免因rosbag录制过程漫长而增加建图时间。

## 实现的方式
1. 指定从ld06.lua加载参数。
2. ld06_mapping.launch 文件将rosbag里的horizontal_laser_2d话题重映射为cartographer_offline_node接收的scan话题。
3. 启动ld06_offline_node.launch文件

## 使用方法
`roslaunch cartographer_ros ld06_mapping.launch bag_filenames:='${DATA}/ld06/taiyanggong_f10_2020_10_29.bag'`

## 效果
![rviz 显示](../pictures/ld06_mapping.gif)