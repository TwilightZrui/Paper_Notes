### [220817]Single-shot Foothold Selection and Constraint Evaluation for Quadruped Locomotion


### [220817]Search-based Foot Placement for Quadrupedal Traversal of Challenging Terrain

* 2007-ICRA
* 类型：落足点规划问题 传统方法 search based
* 问题：给定地形图，当前位姿，receding horizon of n steps.，寻找最优序列：stepping foot, CM heights, body orientations.
  * obj: 与默认构型偏差最小
  * sub:
    * 质心位于支撑三角形内
    * 从foot position  = M * base position 变换有解
  * 规划出的结果，是在指定方向上的一个序列
* 用搜索树的方法，expand node，寻找落足点，质心位置，身体旋转的组合。
* 这个问题的搜索是多步规划
* 尽管这篇论文解决的落足点规划场景是比较单一的，假设比较强烈，但是还是覆盖了落足点规划几个核心的部分：
  * 落足点规划应该是个mutil-step规划问题，one-step规划是比较短视的，不合理的。规划的结果应该是一个序列，序列之间的关系应该是最优的
    * 【序列是怎么做到的？如何把我现在的工作中变成序列】
  * 落足点规划，对于足端的规划，是应该要考虑base状态可达性的，也就是说foot position  = M * base position 变换有解

### [220816]Locomotion Policy Guided Traversability Learning using Volumetric Representations of Complex Environments

* 2022-IROS
* [vedio](https://www.youtube.com/watch?v=GGQ72tbAq0E&ab_channel=RoboticSystemsLab%3ALeggedRoboticsatETHZ%C3%BCrich)
* 地图表示：volumetric / 3D voxel-occupancy map
* 训练了一个sparse CNN, predict 可行性cost
* 为什么用voxel map，不用elevation map，这篇文章要解决多层障碍物的问题吗
* 关注下语义信息到底是怎么结合进机器人导航问题中的
* 不太懂稀疏CNN是怎么专门用于解决这个问题的，创新性在网络吗
* 

### [220805] Where Should I Walk? Predicting Terrain Properties From Images Via Self-Supervised Learning

* learning

### [220804] Deep Convolutional Terrain Assessment for Visual Reactive Footstep Correction on Dynamic Legged Robots

* learning

### [220803]Safe Robot Navigation Via Multi-Modal Anomaly Detection

* learning

### [220802] Fast and Continuous Foothold Adaptation for Dynamic Locomotion through CNNs

* learning

### [220801] Reactive Trotting with Foot Placement Corrections through Visual Pattern Classification

* learning

# legs and wheels

### [220801]Offline motion libraries and online MPC for advanced mobility skills

* legs and wheels

### [220801]Rolling in the Deep - Hybrid Locomotion for Wheeled-Legged Robots Using Online Trajectory Optimization

* legs and wheels

# terrain

### [220801]TAMOLS: Terrain-Aware Motion Optimization for Legged Systems

# Sub

### Rough Terrain Navigation for Legged Robots using Reachability Planning and Template Learning

### RESULTS AND LESSONS LEARNED FROM THE FIRST FIELD TRIAL OF THE ESA-ESRIC SPACE RESOURCES CHALLENGE OF TEAM GLIMPSE

# Foothold

### Foothold Evaluation Criterion for Dynamic Transition Feasibility for Quadruped Robots
