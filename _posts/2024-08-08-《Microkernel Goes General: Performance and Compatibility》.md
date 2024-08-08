---
layout: mypost
title: 《Microkernel Goes General： Performance and Compatibility in the HongMeng Production Microkernel》
categories: [论文阅读]
---
# 《Microkernel Goes General： Performance and Compatibility in the HongMeng Production Microkernel》

**[原文](osdi24-chen-haibo.pdf)**

- 商业实用性：兼容Linux API/ABI以复用其生态（程序和驱动）

- 与传统微内核相比的优化点：
    - IPC
    - capability-based access control
    - userspace paging

- 性能方面遇到的问题：
    - per-invocation IPC
    - IPC frequency, state double bookkeeping among OS services
    - capabilities that hide kernel objects

- 解决方案：
    - differentiated isolation classes
    - flexible composition, policy-free kernel paging, address-token-based access control

## 1. Introduction

**微内核的特征**：微内核将内核中的功能最小化，并将文件系统和设备驱动程序等组件移动到隔离良好、特权最低的操作系统服务中，从而实现了比Linux等单片内核更好的可靠性、安全性和可扩展性。

**单内核（Linux）存在的不足**：
- 像Linux这样的单内核在服务器和云等通用场景中占主导地位，但越来越多的新兴场景，如智能汽车和智能手机，除了良好的性能外，还需要更好的安全性、可靠性和可扩展性，而Linux则不太适合。
- 虽然Linux是通用的，但它更多地向服务器和云发展，使其他场景变得不那么有益。

**微内核（SOTA）存在的不足**：
- SOTA微内核主要针对一些特定领域，例如嵌入式和安全关键领域。它们通常使用静态资源分区和分配，缺乏运行商用现成应用程序的通用操作系统功能。

**微内核改造为通用OS内核面临的主要挑战**：
- Compatibility
- Performance: 当前微内核会造成不晓得性能开销，当微内核被更广泛使用时，IPC频率显著增加（智能手机中的IPC频率比路由器高70倍）

*Capability-based access control*：
    *Capability被简单地称为“密钥”，它是指定资源并授权对其进行某种访问的单一事物。该Capability是不可伪造的权威令牌。*
    *假设有一个文件系统，文件A有如下权限:*   
    - 用户U1可以读和写
    - 用户U2可以读
    *在能力模型中，这些权限将由不同的能力来表示：*
    - 能力C1：允许U1对文件A进行读写操作
    - 能力C2：允许U2对文件A进行读取操作
    *这些能力可以被系统安全地分发和管理。若U1需要将读取权限传递给U3，系统可以创建一个新的能力C3，仅包含读取权限，并将其安全地传递给U3。*

**HongMeng做出的关键设计决策：**
- *具有最低权限和良好隔离的操作系统服务的最小微内核。* HM保留了最小化原则，仅在核心内核中保留必要的功能，包括线程调度程序、serial/timer驱动程序和访问控制，并将所有其他组件作为独立的操作系统服务（多服务器）保留在核心内核之外。
- *通过实现 Linux API/ABI 兼容性和高性能驱动程序复用来最大化兼容性。* HM 通过ABI兼容的shim（shim是一个库，它透明地拦截API调用并更改传递的参数、处理操作本身或将操作重定向到其他地方。Shims 可用于支持较新环境中的旧API，或较旧环境中的新API。shim还可以用于在与开发目的不同的软件平台上运行程序。）来实现完全的Linux API/ABI兼容性，该shim识别并将Linux系统调用重定向到IPC。
- *通过结构性支持优先考虑性能。* HM实现了灵活的组合，以分层放松受信服务之间的隔离，从而最小化IPC开销，并将紧密耦合的服务合并以最小化IPC频率并消除在性能需求场景中的状态双重记账，同时在安全关键场景中保持分离它们的能力。HM还使用基于地址令牌的高性能访问控制来补充基于能力的访问控制，从而促进高效协作操作，例如无策略的内核分页。