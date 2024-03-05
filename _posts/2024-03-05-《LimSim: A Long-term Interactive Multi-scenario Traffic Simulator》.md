---
layout: mypost
title: 《LimSim:A Long-term Interactive Multi-scenario Traffic Simulator》
categories: [论文阅读]
---
## LimSim: A Long-term Interactive Multi-scenario Traffic Simulator
**[源代码](github.com/PJLab-ADG/LimSim)**
**[原文](LimSim_A_Long-Term_Interactive_Multi-Scenario_Traffic_Simulator.pdf)**

### INTRODUCTION

1. 交通仿真器的作用：
    - 成为数字孪生城市建设中的关键要素
    - 面向场景的仿真在自动驾驶的开发和测试中也扮演着重要角色

2. 仿真器的类型
    - 基于流量的仿真器
        - 可以反映现实世界交通系统的特征并分析各种交通流条件
        - 但它们往往简化了车辆运动行为，未考虑多车辆交互和运动学约束
    - 基于车辆的仿真器
        - 旨在构建虚拟道路场景以验证开发的算法性能
        - 背景交通流的生成依赖于手动编辑场景或使用收集的道路数据，这无法准确复现实际场景的特征
    - 基于数据的仿真器
        - 通过从数据集中提取车辆运动特征，这些仿真器允许车辆与背景交通流进行交互
        - 数据集通常提供零散的小规模场景，导致这些仿真器无法进行长期连续的仿真

3. 介绍**LimSim**
    - 我认为比较trick的创新点在于"The range of microscopic simulation is defined by the scenario area. Vehicles outside this area are controlled by classic traffic flow models, while those within the range undergo precise decision-making and planning processes"
    - LimSim将整个道路网络的宏观交通流量模拟与微观车辆运动模拟结合在一起，在特定情况下，提供了交通行为的全面表示

### RELATED WORK
对三类仿真器的近期具体研究成果进行概述

*C. Data-based simulators* 看不太懂

基于数据的模拟器可以从自然驾驶环境中隐式学习多车辆交互，并模拟车辆之间的互动。然而，这些模拟依赖于收集的数据集，使得难以编辑和扩展场景，导致了场景碎片化的问题
