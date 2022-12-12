### [221212]Navigation Planning for Legged Robots in Challenging Terrain

* 2016-IROS
* path plan:rrt*
* OMPL to smooth the solution path

### [220919]Perceptive Locomotion through Nonlinear Model Predictive Control

* 2022-TRO
* 用SDF做碰撞检测
* 与其他工作不同（把整个地形考虑进优化问题？），用凸区域作为足端约束，只有合适的
* 可以实现动态gait
* 感知处理
  * A：Filtering & Classification
    * 1.inpainting：用临近最小值填补elevation map的空洞
    * 2.均值滤波，减小噪声，移除离群点
    * 3.setppablity 分类：inclination roughness甲醛
  * B:Plane Segmentation：找了一块可踏的平面
    * 用凸包算法算了个平面
    * 保护措施：如果也给区域找不到平坦的通行于，用RANSAC找
    * 保护措施：如果区域大小够，那就提供safe margin，如果不够，就不提供了。
    * 不拒接小可行域，在极端情况下比较重要

### [220919]Coupling Vision and Proprioception for Navigation of Legged Robots

* 2022-CVPR
* 外部感知和视觉结合
* 考虑运动能力，能不能走
* 三个部分：
  * 以速度为条件的步行策略：用RL，让机器人用不同速度行走
  * 安全顾问模块
  * 规划模块，它们一起协同利用视觉和本体感觉来为有腿机器人导航。
* pipeline

利用2D相机生成occupancy map和对应的到达目标的cost map。利用FMM（Fast Marching Method）方法生成目标路径。在运动过程中摔倒预测器会根据地形特点生成速度限制，如果预测可能会摔倒，safety advisor会将最大速度限制减小，否则会增加最大速度限制。碰撞检测模块如果检测碰撞。safety advisor模块会在生成的cost map中增加9cm*3cm固定大小表示障碍物，再进行重新路径规划。

safety advisor模块中的摔倒预测模块和碰撞检测模块是两个结构相同的网络。利用一个线性层将状态和动作映射为32维向量x，然后使用3层1维卷积得到扁平化特征，将扁平化特征输入到2层MLP含有8个隐藏单元和一个sigmoid函数得到一个预测概率值。

障碍物检测模块：通过收集机器人行走/碰撞障碍物的数据，以在线方式训练模块。

摔倒检测模块：收集连续t~t+49个x和机器人状态y，如果机器人在t+100时候摔倒为1，否则为0。该模块也以在线方式进行训练，训练过程中在规定的范围中随机采样环境的摩擦系数，线/角速度和有效载荷值。

其中**适应模块**与**基础策略**分别是他们前两个工作[1][2],

**适应模块**的输入是前20个机器人历史状态x和对应的动作，输出一个8维向量，表示外部环境向量。在仿真环境中外部环境设置是已知的，所以可以利用监督学习进行训练。

**基础策略**的输入是30维感知状态x，期望的线速度和角速度，以及前一时刻的12维动作和外部信息的8维向量，输出12维关节角度。利用PPO训练方法训练网络。

[1] Minimizing energy consumption leads to the emergence of gaits in legged robots

[2] RMA: Rapid Motor Adaptation for Legged Robots

### [220918]Visual-Locomotion: Learning to Walk on Complex Terrains with Vision

* 2021-CORL
* RL
* 作者：Wenhao Yu1, google
* 感知+落足点规划用强化学习方法替代了
* 把感知和motion planning 模块用NN代替
* 结构：
* a highlevel vision policy and a low-level motion controller.
  * The high-level vision policy takes two **depthimages** and **outputs the desired pose of the robot’s base and foothold placements** for all swing legs,thereby eliminating the need for a complex SLAM algorithm.（base pose 和落足位置）
  * The low-level locomotion controller takes the high-level vision policy output, and **computes the target motor positions and torques to achieve the desired states.（把目标转化为电机位置和力矩）**
* 文章的合理性体现：
  * 规划了long horizen foothold
  * 规划了body pose 和速度
* Reward:
  * 奖励：COM 加速度，希望x方向以最大速度前进
  * 惩罚：希望不要在y方向有位移
  * 惩罚：yaw角，希望不要在超其他方向前进
* 终止条件：
  * 机器人失去平衡了
  * 迈步在障碍物范围内了
  * 机器人达到了一种无效关节构型
* Motion Controller
  * * 这篇文章用的是MIT cheetah 3
    * 把落足位置和base target转化成motor position and torque commands
    * 分开控制swing & stance legs
      * swing leg controller：
        * 起始点到目标点插值
        * 落足位置通过IK转换成关节角
        * 关节角用PD跟踪
      * stance leg controller：
        * 通过计算足端和地面之间的接触力，实现base所需的目标位置和速度
        * 把机器人动力学近似成质心动力学，该问题被建模成MPC问题
        * 优化得到的接触力被映射成站立腿的关节力矩（痛jacobian transpose）
* Sim-to-real Transfer：
* 强化学习过程细节：

![1663949610925](image/quadruped/1663949610925.png)

![1663952526932](image/quadruped/1663952526932.png)

### [220918]Reliable Trajectories for Dynamic Quadrupeds using Analytical Costs and Learned Initializations

* 2020 ICRA
* 对Towr 库的改进

### [220917]Automatic Gait Pattern Selection for Legged Robots

* 2020IROS
*

### [220917]DeepGait: Planning and Control of Quadrupedal Gaits using Deep Reinforcement Learning

### [220917]Reinforcement Learning with Evolutionary Trajectory Generator: A General Approach for Quadrupedal Locomotion

* 2022 RAL
* 百度
* [RL]

### [220911]Learning Locomotion over Rough Terrain using Terrain Templates

* 2009
* 问题定义：
  * 每个候选落足点是由一个特征向量表征的
* [RL]

### [220827]Online Walking Motion and Foothold Optimization for Quadruped Locomotion

* 2017-ICRA
* Alexander W.  COM +落足点  一个优化问题
* 从模型角度解释了，为什么要COM foothold plann要耦合

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

### [220801]TAMOLS: Terrain-Aware Motion Optimization for Legged Systems

* 2022-TRO

落足点规划问题解耦

应该关注的是，落足点是怎么嵌入优化问题的，

怎么嵌入能把落足点结合地形解出来，这应该是创新点

# Sub

### Rough Terrain Navigation for Legged Robots using Reachability Planning and Template Learning

### RESULTS AND LESSONS LEARNED FROM THE FIRST FIELD TRIAL OF THE ESA-ESRIC SPACE RESOURCES CHALLENGE OF TEAM GLIMPSE

# Foothold

### Foothold Evaluation Criterion for Dynamic Transition Feasibility for Quadruped Robots
