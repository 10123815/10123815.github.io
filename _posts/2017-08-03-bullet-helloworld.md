---
layout:     post
comments:   true
title:      "bullet入门"
date:       2017-08-03
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:
        - 物理引擎
        - bullet
---

#### Hello World

1. 创建btDefaultCollisionConfiguration，主要是内存管理
2. 创建btCollisionDispatcher，为凸凸、凸凹的碰撞检测提供算法，并提供调用回调函数，用于传递碰撞信息？
3. 创建btBroadphaseInterface，里面应该包含一些粗略阶段的算法算法。Hello World中使用了btDbvtBroadphase。
  + btDbvt是一个可以快速动态更新的aabb树，看注释除了btDbvtBroadphase还能用在柔体碰撞检测，是一颗二叉树
  + btDbvtBroadphase应该是递归的判断btDbvtNode的两个孩子是否有overlapping，有的话调碰撞处理函数？
4. 创建btSequentialImpulseConstraintSolver，约束分析？不懂
5. 创建物理世界并设置重力
6. 创建一些刚体并加入物理世界。在这之前要创建碰撞体，碰撞体应该保存起来，并在刚体之间重用
  1. 一个btBoxShape作为地面
    + 地面的重量是0，是静态的
    + 创建btTransform和btDefaultMotionState并将两者联系到一起
      + btTransform表示位置信息
      + btDefaultMotionState，wiki上的解释 MotionStates are a way for Bullet to do all the hard work for you getting the objects being simulated into the rendering part of your program. 简单说就是Motion是一个接口，bullet将物理世界中因为力导致的位移通过Motion传递给你自己的程序。至于运动学物体，你在程序里控制他，Motion将他的位移传递至bullet来计算碰撞检测
    + btRigidBodyConstructionInfo里面是一些参数，包括刚才的重量、motion，仅用来初始化刚体。可以创建很多刚体，并只通过一个btRigidBodyConstructionInfo初始化他们
    + 最后用btRigidBodyConstructionInfo创建刚体btRigidBody
  2. 创建一个btSphereShape
    + 一个动态物体，重力、惯性都不为0
    + 创建btTransform和btDefaultMotionState
    + 创建btRigidBodyConstructionInfo
    + 最后用btRigidBodyConstructionInfo创建刚体btRigidBody
7. 循环
  + stepSimulation的注释写到：stepSimulation proceeds the simulation over 'timeStep', units in preferably in seconds. By default, Bullet will subdivide the timestep in constant substeps of each 'fixedTimeStep'. In order to keep the simulation real-time, the maximum number of substeps can be clamped to 'maxSubSteps'. 大意是，bullet按照timeStep去模拟物理世界，为了更平滑，timeStep被分成fixedTimeStep，最多分成maxSubSteps个，中间的这些fixedTimeStep的状态经过差值得到。如果maxSubSteps设为0，就不划分timeStep。
  + 迭代全部的刚体，通过motion得到其transform，并将位置打印出来
  + 有些碰撞体不是刚体，也可以直接获得位置
8. 各种delete