---
layout: mypost
title: 《Microscopic Traffic Simulation using SUMO》
categories: [论文阅读]
---
## Microscopic Traffic Simulation using SUMO
**[原文](Microscopic_Traffic_Simulation_using_SUMO.pdf)**

### INTRODUCTION

##### 交通仿真工具的类别
- 宏观：模拟平均车辆动态，如交通密度
- 微观：每辆车及其动态被单独建模
- 中观：宏观和微观模型的混合
- 亚微观：每辆车以及车内功能都被明确模拟，例如换挡

本文着重介绍微观交通仿真

### WORKFLOW
着重论述交通模拟的工作流程：
1. 建立交通网（NETWORK SETUP）
2. 扩展网，例如公共交通专用道和信号灯（NETWORK SETUP）
3. 生成路径需求，即生成车辆实际进行行动的交通路径（Demand Modeling and Adaption）
...

### EXAMPLE: BOLOGNA SCENARIO
一个博洛尼亚街道的例子
...

### NETWORK SETUP

##### 介绍SUMU网络
SUMO网络由节点和单向边组成，表示街道、水路、轨道、自行车道和人行道。每条边的几何形状由一系列线段描述，包含一个或多个平行运行的车道。

为了确保网络表示一致，SUMO网络是使用NETCONVERT和NETEDIT应用程序创建的。
NETCONVERT是一个命令行工具，可用于从不同数据源（如OpenStreetMap（OSM）、OpenDRIVE、Shapefile）或从其他仿真器（如MATSim和Vissim）导入道路网络。NETCONVERT的一个关键特性是启发式地完善缺失的网络数据，以实现微观仿真所需的详细级别（例如为OSM网络合成交通灯计划、让行规则和交叉口几何形状）。

NETCONVERT？启发式？

由于输入数据与微观仿真所需的详细级别之间经常存在不匹配，网络和基础设施准备往往是一项具有挑战性的任务。可使用**NETCONVERT**

### Demand Modeling and Adaption

*A. Demand importing and generation*

需求建模即是生成一条有效的交通路径，介绍了很多基于网状路线静态结构特征生成交通路径的方法和工具

DFROUTER：主要概念是根据高速公路和相关匝道以及互通的检测到的流量，在每个检测器上生成路径。

*B. Demand adaptation*

对于规划好的路径，其可能并不太符合实际交通情况。即交通需求数据具有有限可用性的问题，在该章提出了三种解决方法。（基本都是使用外部开源程序修改或者丢弃路线，以调整不同路径的流量及存在性）

### Multi- and Intermodal Traffic