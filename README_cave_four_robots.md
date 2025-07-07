# 四个机器人Cave地图使用说明

## 概述

这个包提供了一个包含四个机器人的cave地图仿真环境，所有机器人共享同一个tf_tree。

## 机器人配置

地图中包含四个不同类型的机器人：

1. **robot_0** (红色) - 带激光和相机的Pioneer2DX机器人
   - 位置: [-7, -7, 0, 45°]
   - 传感器: 激光扫描仪 + 深度相机

2. **robot_1** (绿色) - 带激光和相机的Pioneer2DX机器人
   - 位置: [-5, -4, 0, 225°]
   - 传感器: 激光扫描仪 + 深度相机

3. **robot_2** (青色) - 双激光Pioneer2DX机器人
   - 位置: [-5, -6, 0, 225°]
   - 传感器: 两个激光扫描仪

4. **robot_3** (橙色) - 带激光和相机的Pioneer2DX机器人
   - 位置: [-3, -7, 0, 90°]
   - 传感器: 激光扫描仪 + 深度相机

## 启动方法

### 方法1: 使用专用launch文件
```bash
ros2 launch stage_ros2 cave_four_robots.launch.py
```

### 方法2: 使用通用launch文件并指定world
```bash
ros2 launch stage_ros2 demo.launch.py world:=cave_four_robots one_tf_tree:=true
```

## 关键特性

### TF树配置
- `one_tf_tree=true`: 所有机器人共享同一个tf树
- 所有机器人的tf帧都发布到 `/tf` 和 `/tf_static` 话题
- 便于多机器人协作和导航

### 话题结构
每个机器人的话题都以其名称作为前缀：
- `/robot_0/odom` - 机器人0的里程计
- `/robot_0/base_scan` - 机器人0的激光扫描数据
- `/robot_0/image` - 机器人0的RGB图像
- `/robot_0/depth` - 机器人0的深度图像
- 其他机器人类似

### RViz配置
- 包含所有四个机器人的可视化配置
- 显示每个机器人的里程计、激光扫描和相机数据
- 支持TF帧的可视化

## 自定义配置

### 修改机器人位置
编辑 `src/stage_ros2/world/cave_four_robots.world` 文件中的 `pose` 参数。

### 修改机器人类型
在world文件中可以更改机器人类型：
- `pioneer2dx_with_laser_and_camera` - 带激光和相机
- `pioneer2dx_with_two_laser` - 双激光
- `pioneer2dx_with_laser` - 仅激光

### 添加更多机器人
在world文件中添加更多机器人定义，并在rviz配置文件中添加相应的显示配置。

## 注意事项

1. 确保所有机器人的位置不会相互碰撞
2. 使用 `one_tf_tree=true` 参数确保TF树正确配置
3. 如果需要单独控制某个机器人，可以使用相应的命名空间话题
4. 地图基于cave.png位图文件，确保该文件存在于正确位置

## 故障排除

如果遇到问题：
1. 检查world文件中的机器人名称是否唯一
2. 确认所有引用的位图文件存在
3. 验证launch文件中的参数设置正确
4. 检查RViz配置是否与机器人数量匹配 