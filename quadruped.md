### [220827]Online Walking Motion and Foothold Optimization for Quadruped Locomotion

### [220818]Single-shot Foothold Selection and Constraint Evaluation for Quadruped Locomotion

* 2019-ICRA
* 规划最优落足点的原则：
  1、落足点应该远离边缘、陡峭台阶等危险地带
  2.选定的落足点应该在工作空间内，不要超过运动学极限
  3、避免自碰撞、和地形碰撞
* 数据集采集：
  * 在仿真中采集数据，用迁移学习方法用到实物狗上
  * 在仿真生成一个12*12地图
  * 由于机器人是对称的，所以训练右腿的数据，用到左腿上，因此只用收集右侧两条腿的数据，训练两个模型
  * 训练数据生成：如何评价仿真环境中落足点分数：
    * 硬约束：（）255
      * 工作空间极限
      * 自碰撞和地面碰撞（用碰撞检测库）
      * **【运动学有解？】**
    * 软约束：
      * 运动学稳定裕度
      * 地形代价
* network:
  * model: ERF
  * cost分了14个类
  * optimized objective: cross-entropy loss and regularization loss
* 推理：
  * cfinal = c f +k ·dn （k人工调）
  * Pipeline: 拿到子图、转化为normalized image、计算距离，推理
* 接入了控制框架中：![1660893302957](image/quadruped/1660893302957.png)
* 认识：
  * 用学习方法端到端做落足点规划问题，涉及到语义分割领域的内容比较多，在场景层面，像素级别的语义分割
  * 网络针对化的改进理解的不够好
* 缺点：
  * 没有在生成网络时，考虑姿态可达性。
* **可以改进的部分：**
  * 学习方法是否可以把姿态可达性也考虑进网络的生成traning set过程中，即在约束中加这部分
  * 或者，学习直接学出来下一个步态周期，base目标位姿
  * 我现在的方法，没有稳定裕度接口，远离障碍物边缘是个很重要的事情，程序实现中应该加入这一部分
  * 我应该做出来一版用优化方法解决的，作为对比实验
  * 问一下控制组，如果落足点规划模块把身体位姿一起计算出来，那边能实现吗

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

落足点规划问题解耦

应该关注的是，落足点是怎么嵌入优化问题的，

怎么嵌入能把落足点结合地形解出来，这应该是创新点


# Sub

### Rough Terrain Navigation for Legged Robots using Reachability Planning and Template Learning

### RESULTS AND LESSONS LEARNED FROM THE FIRST FIELD TRIAL OF THE ESA-ESRIC SPACE RESOURCES CHALLENGE OF TEAM GLIMPSE

# Foothold

### Foothold Evaluation Criterion for Dynamic Transition Feasibility for Quadruped Robots
