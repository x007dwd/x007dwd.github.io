---
layout:     post
title:      "Guidance of DJI"
subtitle:   " \"博学之审问之慎思之明辨之笃行之。\""
date:       2017-3-20 21：19
author:     "Bobin"
tags:
    - DJI
---
Guidance是一个不错的SDK，提供了多种类型处理器的，跨平台的支持。
在使用的时候，存在说明文档介绍了开发方法，这里简要介绍一下在linux下如何完成ROS包的开发，一步一步完成开发。
## 文档结构
ROS包还是以往的特点，体积小，针对性强。这个SDK的特点就是不开源。存在一些so文件用来完成guidance和PC之间的通信的功能。在使用的时候订阅ROS topic即可。这个包很小，代码也很短。源代码中编译了三个node，分别是guidanceNode，guidanceNodeTest，guidanceNodecalibration.

## guidanceNode
首先是一段advertise ROS toplics，初始化代码，读取sensor status、相机的校准参数、然后开始传输数据。SDK的特点是要选择传输那个IMU、Camera等等，以便于优化传输带宽。主要的函数为set_sdk_event_handler，这里制定所需的回调函数。
回调函数中publish 相关messge，分别是

| name | type | string|
|-------|-----|-------|
|depth_image_pub | sensor_msgs::Image | /guidance/depth_image |
|left_image_pub | sensor_msgs::Image | /guidance/left_image |
|right_image_pub | sensor_msgs::Image | /guidance/right_image |
|imu_pub | geometry_msgs::TransformStamped | /guidance/imu |
|velocity_pub | geometry_msgs::Vector3Stamped | /guidance/velocity |
|obstacle_distance_pub  | sensor_msgs::LaserScan | guidance/obstacle_distance|
|ultrasonic_pub | sensor_msgs::LaserScan | /guidance/ultrasonic |
