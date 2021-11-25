---
layout: post
title: Artificial Intelligence for Detection, Estimation, and Compensation of Malicious Attacks in Nonlinear Cyber-Physical Systems and Industrial IoT
description: "还没看完 11-3"
subtitle: ""
data: 2021-11-2
author: Fat-Pman
header-img: img/post-bg/3.jpg
catalog: true
tags: Papers, Federated Learning, Defense
---

### Artificial Intelligence for Detection, Estimation, and Compensation of Malicious Attacks in Nonlinear Cyber-Physical Systems and Industrial IoT

<font color=blue>对非线性物理系统以及工业互联网中的恶意攻击进行检测，评估，修正的人工智能</font>

#### Abstract 

本文提出了一种（智能-经典）混合控制方法，用于对非线性网络物理系统(CPS)和工业物联网系统的网络攻击进行重建和补偿，其中CPS与IIoT通过共享通信网络工作。本文将n阶非线性系统看作网络攻击仅存在于前向信道的CPS模型。同时开发了一种智能经典控制系统来补偿网络攻击。设计了神经网络(NN)作为攻击检测的智能评估器，并设计了<font color=red>基于可变结构控制方法</font>的经典非线性控制系统来补偿攻击的影响，以及控制系统在跟踪应用中的性能。在该策略中，应用非线性控制理论来保证系统在受到攻击时的稳定性。该策略采用<font color=red>高斯径向基函数NN</font>对网络系统的网络攻击进行在线估计和重构。由李雅普诺夫函数导出了智能估计器的<font color=red>自适应律</font>。仿真结果验证了该策略在汽车巡航控制中的有效性和可行性。

<font color=red>1.目前主要关注在于自适应律以及可变结构</font>

#### Introduction

信息物理系统(CYBER-PHYSICAL systems, CPS)是计算、网络和物理过程的集成。CPS由与物理世界及其过程相联系的协作计算对象组成。CPS是物联网(IoT)的近亲，但由于与物理实体[1]-[3]关联的独特属性（<font color=red>应该是cps对物理实体控制的更加密切</font>），与物联网有些差别。在自动驾驶汽车、机器人手术、智能建筑、智能电网、智能制造和植入医疗设备中，有许多CPS的例子。cps的一个突出特性是其广泛性和普遍性，这是制造技术实现无所不在的关键因素。目前，在工业物联网[4]-[7]和面向服务架构的背景下，制造业普遍存在，以支持智能控制解决方案.

#### Control Problem Formulation For a Class of NonLinear CPSs

在本文中，控制系统的目的是在假定CPS易受攻击的情况下，提高其安全性和可靠性。这是通过设计能够在攻击场景下保持稳定性和性能的控制算法来实现的。考虑一类被描述为标准(affine-<font color=red>可以理解为映射</font>)非线性时不变连续时间系统的n阶非线性cps：

$$
\large{x^{(n)}(t)=f(x,\dot x,\ddot x,...,x^{(n-1)})+g(x,\dot x,\ddot x,...,x^{(n-1)})u(t)}
$$

$$
\large{y(t)=C[x,\dot x,\ddot x,...,x^{(n-1)}]^T}
$$

其中f和g是已知的实连续非线性状态函数，它们是CPS的动态函数。y∈R为输出向量，u∈R为CPS的控制输入。

![System](./img/20211102/1.jpg)

假设系统是单输入单输出(SISO)。CPS输入会受到恶意攻击的威胁，通过增加其控制输入值来改变控制输入[见图1(a)]。因此，(1)会改为

$$
\large{x^{(n)}(t)=f(x,\dot x,\ddot x,...,x^{(n-1)})+g(x,\dot x,\ddot x,...,x^{(n-1)})(u(t)+u_a(t))}
$$

$$
\large{y(t)=C[x,\dot x,\ddot x,...,x^{(n-1)}]^T}
$$
同样上式也可以写成：

$$
\large{x^{(n)}(t)=f(x,\dot x,\ddot x,...,x^{(n-1)})+g(x,\dot x,\ddot x,...,x^{(n-1)})u(t)+A(t)}
$$

$$
\large{y(t)=C[x,\dot x,\ddot x,...,x^{(n-1)}]^T}
$$

我们假设物理系统存在影响状态的附加标量扰动。考虑到这种可能性，上式可以重写为

$$
\large{x^{(n)}(t)=f(x,\dot x,\ddot x,...,x^{(n-1)})+g(x,\dot x,\ddot x,...,x^{(n-1)})u(t)+A(t)+d(t)}
$$

$$
\large{y(t)=C[x,\dot x,\ddot x,...,x^{(n-1)}]^T}
$$

其中，A(t)部分可以看作直接破坏CPS的攻击，d(t)能表示为任何可能有界的外部扰动。

$$
\large{\dot x_1(t)=x_2(t)}\\
\large{\dot x_2(t)=x_3(t)}\\...\\
\large{\dot x_n-1(t)=x_n(t)}\\
\large{\dot x_n(t)=f(X)+g(X)u(t)+A(t)+d(t)}\\
\large{y(t)=CX}\\
X=[x=x_1,\dot x=x_2,...,x^{(n-1)}=x_n]^T
$$

CPS在以下假设下运作。

1)系统为SISO。因此，假设C =[1,0，…，0]T。

2)物理系统的数学模型是已知的。

3)状态(f,g)向量不可用，只有输出可用且可测量。

4)当需要计算状态向量时，可以利用实测输出和物理系统的数学模型来得到。

5)反馈环节安全。因此，输出测量是可靠的。

6)网络攻击只发生在前向链路和控制输入上，通过添加攻击向量扰乱控制信号，如图1(a)所示。

7)物理系统在存在影响状态的外部扰动时工作。

8)物理系统和传感器无故障

请注意，有几种类型的攻击可能威胁到cps，它们甚至可能影响系统的物理或网络组件。在本文中，欺骗攻击和隐身攻击分别可能影响系统。欺骗攻击指的是可能损害控制包或测量的完整性，并通过改变传感器和执行器[8]的行为来实现。在隐形攻击中，攻击者通过物理上篡改单个仪表或访问某些通信通道来修改某些传感器的读数。

本文将攻击的信号模型描述为
$$
A(t)=\zeta(X,t)
$$

其中X是状态向量，t是时间。函数ζ是未知的，但有界。虽然确切的函数被认为是未知的，但我们假设攻击函数是可预测的。此外，在设计经典VS控制系统时，模型函数的边界(以及它们所有的时间导数)是已知的。|ζ(X, t)| < β 0， |̇ζ(X, t)| < β 1…， |ζ(n)(X, t)| < β n