# rosplay use current time

## 实现的功能
回放rosbag, 同时将日志的时间戳由日志生成的时间更改为系统的当前时间。回放过程中不改变消息的topic, frame_id, 回放速度。

该功能能够将日志回放的过程模拟成实时过程，使得能够使用同一个launch文件验证实时和离线场景下的算法。

## 实现的方式
1. 将cartographer_ros自带的cartographer_dev_rosbag_publisher写成launch文件.
2. cartographer_dev_rosbag_publisher 由cartographer_ros/dev/rosbag_publisher_main.cc编译生成
3. 在configuration_files中新增laser_scan.rviz配置文件，使得rviz默认显示/horizontal_laser_2d.

## 使用方法
`roslaunch cartographer_ros rosplay_use_current_time.launch bag_filename:=${DATA}/ld06/taiyanggong_f10_2020_10_29.bag`


## 效果
![rviz 显示](../pictures/rosplay_use_current_time.png)