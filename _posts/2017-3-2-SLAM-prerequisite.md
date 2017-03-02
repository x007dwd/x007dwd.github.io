---
layout:     post
title:      "SLAM预备知识"
subtitle:   " \"好察非明，能察能不察之谓明；必胜非勇，能胜能不胜之谓勇。\""
date:       2017-3-2 22：59
author:     "Bobin"
tags:
---

# 视觉slam
先说视觉这块，首先射影几何的一些内容相机模型，单视几何，双视几何和多视几何。这些内容可以在http://www.robots.ox.ac.uk/~vgg/hzbook/这本书中找到。英文版的，另外中科院的吴福朝编著的“计算机视觉中的数学方法”也很好，他涵盖了上述了MVG in CV book中的大部分内容，强烈安利。

然后是一些视觉特征，这方面就是一些特征，描述子，匹配相关等。见SIFT，ORB、BRISK、SURF等文章。

数学方面首先是三维空间的刚体运动，参考机器人学，
关于优化，SLAM中的优化方法十分基本，参考高斯牛顿，LM，结合稀疏线性代数，其实用的时候会使用一种g2o的图优化库或者ceres。

最难的应该算是李群和李代数，这方面可以参考book [state estimation for Robotics](asrl.utias.utoronto.ca/~tdb/bib/barfoot_ser15.pdf)。当然不想看书的话可以参考博客http://www.cnblogs.com/gaoxiang12/tag/%E6%9D%8E%E4%BB%A3%E6%95%B0/。

为了看论文的时候能够比较流畅，还应该具备一些概率论的知识，这里推荐book[Probabilistic Robotics](http://www.probabilistic-robotics.org/) [pdf](https://docs.ufpr.br/~danielsantos/ProbabilisticRobotics.pdf)

话说高翔博士近期完成一本SLAM的入门book，有理论有实践，写的不错，推荐包含了上述在视觉slam需要的所有基础知识，真是造福大众啊。详细研读此书，以后读各种论文就不会显得那么吃力了吧。最后列举一些玩slam的一些必备工具和相关资源。
## tools
1. ubuntu, install, cmake, bash, vim, qt(optional).
2. OpenCV install, read the opencv reference manual and tutorial
3. ros, [install](http://wiki.ros.org/ROS/Installation), [tutorial}(http://wiki.ros.org/ROS/Tutorials).
4. python. 可以使用pycharm,作为IDE.

为什么使用ubuntu？因为大家的代码，全是用linux，而且很多使用ros的，ros一定是要Linux的，同时还要cmake。Ubuntu是比较适合初学Linux的人，非常好用

## somethind about Calibration
1. [opencv camera Calibration](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html)
2. [matlab camera Calibration toolbox](http://www.vision.caltech.edu/bouguetj/calib_doc/)
3. [svo camera Calibration](https://github.com/uzh-rpg/rpg_svo/wiki/Camera-Calibration)
4. [ros wiki camera Calibration](http://wiki.ros.org/camera_calibration)

为什么要标定相机呢，因为slam的模型中假设 相机的内参数是已知的，因此有了这个内参数我们才能正确的初始化slam系统。

## ROS
### ros usually used pakcage
1. [svo](https://github.com/uzh-rpg/rpg_svo/)
2. [orb slam](https://github.com/raulmur/ORB_SLAM2)
3. [ar_tracker_alvar githun page](https://github.com/sniekum/ar_track_alvar) [ros page](http://wiki.ros.org/ar_track_alvar)
4. [ros ptam](http://wiki.ros.org/ethzasl_ptam),原始代码不支持ros, 这里给出ros版本的代码. 原始[代码](https://github.com/Oxford-PTAM/PTAM-GPL)[网站](http://www.robots.ox.ac.uk/~gk/PTAM/)
5. [DSO](https://github.com/JakobEngel/dso)

### ros books
1. Learning ROS for Robotics Programming
2. 机器人操作系统（ROS）浅析
### some blogs about ros
1. http://www.guyuehome.com/page/1


## SLAM基础学习
1. [Multiple View Geometry in Computer Vision](http://www.robots.ox.ac.uk/~vgg/hzbook/)。这本书基本涵盖了Vision-based SLAM这个领域的全部理论基础！读多少遍都不算多！另外建议配合Berkeley的课件学习。（更新：这本书书后附录也可以一并读完，包括附带bundle adjustment最基本的levenberg marquardt方法，newton方法等）．
2. Sparse Matrix，这是大型稀疏矩阵处理的一般办法。可以参考Dr. Tim Davis的课件：[Tim Davis](http://faculty.cse.tamu.edu/davis/welcome.html) ，他的主页里有全部的课程视频和Project。针对SLAM问题，最常用的least square算法是Sparse Levenberg Marquardt algorithm，这里有一份开源的代码以及具体实现的paper：[Sparse Non-Linear Least Squares in C/C++](http://users.ics.forth.gr/~lourakis/sparseLM/)
3. [openSLAM](https://www.openslam.org/)
4. dataset [tum](https://vision.in.tum.de/data/datasets/rgbd-dataset)
5. [PCL](https://github.com/PointCloudLibrary/pcl)
6. [opencv](http://opencv.org/)

## 推荐阅读的书
1. [Multiple View Geometry in Computer Vision](http://www.robots.ox.ac.uk/~vgg/hzbook/)
2. [Probabilistic Robotics](http://www.probabilistic-robotics.org/) [pdf](https://docs.ufpr.br/~danielsantos/ProbabilisticRobotics.pdf)
3. [state estimation for Robotics](asrl.utias.utoronto.ca/~tdb/bib/barfoot_ser15.pdf)
4. [Quaternion kinematics for the error-state KF](www.iri.upc.edu/people/jsola/JoanSola/objectes/notes/kinematics.pdf)
5. 凸优化，https://web.stanford.edu/~boyd/cvxbook/bv_cvxbook.pdf
6. 线性系统理论，https://www.amazon.com/Linear-System-Electrical-Computer-Engineering/dp/0199959579
7. An Invitation to 3-D Vision，https://www.eecis.udel.edu/~cer/arv/readings/old_mkss.pdf
8. Modern Control Systems，https://www.amazon.com/Modern-Control-Systems-12th-Richard/dp/0136024580
9. Rigid Body Dynamics，http://authors.library.caltech.edu/25023/1/Housner-HudsonDyn80.pdf。说实话刚体动力学理论我没有找到特别好的书。但是刚体动力学理论很重要。
10. Feedback Systems: An Introduction for Scientists and Engineers，FBSwiki
11. 《机器学习》，周志华老师的书。
12. 线性估计，https://www.amazon.com/Linear-Estimation-Thomas-Kailath/dp/0130224642



### vision Navigation
- Georg Klein and David Murray, "Parallel Tracking and Mapping for Small AR Workspaces", In Proc. International Symposium on Mixed and Augmented Reality (ISMAR'07, Nara).
- D. Scaramuzza, F. Fraundorfer, "Visual Odometry: Part I - The First 30 Years and Fundamentals IEEE Robotics and Automation Magazine", Volume 18, issue 4, 2011.
- F. Fraundorfer and D. Scaramuzza, "Visual Odometry : Part II: Matching, Robustness, Optimization, and Applications," in IEEE Robotics & Automation Magazine, vol. 19, no. 2, pp. 78-90, June 2012.
doi: 10.1109/MRA.2012.2182810
- A Kalman Filter-Based Algorithm for IMU-Camera Calibration Observability Analysis and Performance Evaluation
- SVO- Fast Semi-Direct Monocular Visual Odometry
- [eth zasl sensor](http://wiki.ros.org/ethzasl_sensor_fusion),
  - Stephan Weiss. Vision Based Navigation for Micro Helicopters PhD Thesis, 2012 pdf
  - Stephan Weiss, Markus W. Achtelik, Margarita Chli and Roland Siegwart. Versatile Distributed Pose Estimation and Sensor Self-Calibration for Autonomous MAVs. in IEEE International Conference on Robotics and Automation (ICRA), 2012. pdf
  - Stephan Weiss, Davide Scaramuzza and Roland Siegwart, Monocular-SLAM–based navigation for autonomous micro helicopters in GPS-denied environments, Journal of Field Robotics (JFR), Vol. 28, No. 6, 2011, 854-874. pdf
  - Stephan Weiss and Roland Siegwart. Real-Time Metric State Estimation for Modular Vision-Inertial Systems. in IEEE International Conference on Robotics and Automation (ICRA), 2011. pdf
  - Simon Lynen, Markus Achtelik, Stephan Weiss, Margarita Chli and Roland Siegwart, A Robust and Modular Multi-Sensor Fusion Approach Applied to MAV Navigation. in Proc. of the IEEE/RSJ Conference on - - Intelligent Robots and Systems (IROS), 2013. pdf
- [orb slam]
  - Raúl Mur-Artal, J. M. M. Montiel and Juan D. Tardós. ORB-SLAM: A Versatile and Accurate Monocular SLAM System. IEEE Transactions on Robotics, vol. 31, no. 5, pp. 1147-1163, 2015. (2015 IEEE Transactions on Robotics Best Paper Award). PDF.
  - Dorian Gálvez-López and Juan D. Tardós. Bags of Binary Words for Fast Place Recognition in Image Sequences. IEEE Transactions on Robotics, vol. 28, no. 5, pp. 1188-1197, 2012.

![](/img/SLAM-prerequiste.png)
## 参考
1. 大疆的YY硕https://www.zhihu.com/question/24492974/answer/29987148
2. https://zhuanlan.zhihu.com/p/22266788
