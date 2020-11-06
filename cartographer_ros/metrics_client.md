# metrics client

## 简述
`cartographer`自带`metrics`模块。该模块负责收集程序运行过程中的各种数据。这些数据被存储成三种格式，分别为`counter`, `histogram`和`gauge`(官方似乎未完成，还在开发中) 。  
`counter`收集了：  
    1. cartographer_sensor_data_total
    2. mapping_global_trajectory_builder_local_slam_results
    3. mapping_constraints_constraint_builder_2d_constraints
    4. mapping_constraints_constraint_builder_3d_constraints
    5. collator_input_total

`histogram`收集了：  
    1. mapping_2d_local_trajectory_builder_scores
    2. mapping_2d_local_trajectory_builder_costs
    3. mapping_2d_local_trajectory_builder_residuals
    4. mapping_constraints_constraint_builder_2d_scores
    5. mapping_3d_local_trajectory_builder_scores
    6. mapping_3d_local_trajectory_builder_costs
    7. mapping_3d_local_trajectory_builder_residuals
    8. mapping_constraints_constraint_builder_3d_scores

`gauge`收集了： 
    1. cloud_internal_map_builder_server_incoming_data_queue_length
    2. mapping_2d_local_trajectory_builder_latency
    3. mapping_2d_local_trajectory_builder_real_time_ratio
    4. mapping_2d_local_trajectory_builder_cpu_real_time_ratio
    5. mapping_2d_pose_graph_work_queue_delay
    6. mapping_2d_pose_graph_work_queue_size
    7. mapping_2d_pose_graph_constraints
    8. mapping_2d_pose_graph_submaps
    9. mapping_3d_local_trajectory_builder_latency
    10. mapping_3d_local_trajectory_builder_voxel_filter_fraction
    11. mapping_3d_local_trajectory_builder_scan_matcher_fraction
    12. mapping_3d_local_trajectory_builder_insert_into_submap_fraction
    13. mapping_3d_local_trajectory_builder_real_time_ratio
    14. mapping_3d_local_trajectory_builder_cpu_real_time_ratio
    15. mapping_3d_pose_graph_work_queue_delay
    16. mapping_3d_pose_graph_work_queue_size
    17. mapping_3d_pose_graph_constraints
    18. mapping_3d_pose_graph_submaps
    19. mapping_constraints_constraint_builder_2d_queue_length
    20. mapping_constraints_constraint_builder_2d_num_submap_scan_matchers
    21. mapping_constraints_constraint_builder_3d_queue_length
    22. mapping_constraints_constraint_builder_3d_num_submap_scan_matchers

`cartographer`通过调用`void RegisterAllMetrics(FamilyFactory *registry)`静态激活以上所有`metrics`  
`cartographer ros`通过使能`collect_metrics`激活以上所有`metrics`并通过`ros::ServiceServer`启动`service`节点。


## 结果
![metric_values](../pictures/metrics_value.gif)

## 结果解释
1. 上述图左侧输出为各项`metrics`的实时输出，右侧有rviz中的实时定位过程。  
2. 左侧实时输出的`metrics`中去掉了部分参数值无效的消息。
3. 在左侧实时输出区域里，每一个`metric`以`metric_name:`开始，直到下一个`metric_name:`结束。每一个metric里包含若干个`score_msg`， `score_msg`包含`label`部分和`data`部分。
4. `label`包含`key`和`value`属性，以字符串输出。
5. `data`分为`counter`, `histogram`和`gauge`三种类型。
6. `mapping_2d_local_trajectory_builder_scores`： 实时点云经过上一帧的rt变换后，需要与`submap`匹配以获得当前帧到`submap`的rt,该过程为[correlative scan matching](https://www.researchgate.net/publication/224557172_Real-time_correlative_scan_matching). 通过配置参数`use_online_correlative_scan_matching`可以选择开启或关闭该功能。该过程计算得到的`score`以`histogram`类型存储于`mapping_2d_local_trajectory_builder_scores：`。
7. `mapping_2d_local_trajectory_builder_costs`： 在使用`correlative scan matching`进行实时点云到`submap`的匹配之后，`cartographer`进一步的会使用ceres将点云匹配到`submap`中，该过程不可设置关闭。ceres求解结果`ceres::Solver::Summary::final_cost` 存储于`mapping_2d_local_trajectory_builder_costs`， 求解出的位姿为观测值。

8. `mapping_2d_local_trajectory_builder_residuals`： 在进行点云匹配到`submap`之前， `cartographer`会计算出位姿的预测值。`distance residuals`为观测值与预测值的距离; `angle residuals`为观测值与预测值的角度差绝对值.


## to be continued