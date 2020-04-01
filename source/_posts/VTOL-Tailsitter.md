---
title: Vertical Takeoff and Landing (VTOL) Tail-sitter UAV
tags: [PhD, Thesis]
date: 2018-10-14 21:24:56
updated: 2019-07-17 21:24:56
categories: project
---
This is the main topic for my [PhD thesis](http://ira.lib.polyu.edu.hk/handle/10397/80315). The aim of this project is to design and optimize a VTOL tail-sitter UAV. The main work can be divided into design, model and control parts.

<img src="/images/jiajia1.jpg" width = "550" height = "300" alt="title" align=center />

<!-- more -->

## Abstract

Unmanned Aerial Vehicles (UAVs) are playing more and more important roles in current society. They can not only be used for entertainment, but also execute missions such as surveillance, cargo delivery, geographic survey, etc. Tail-sitter UAV, one of the configurations that can vertically take off and land (VTOL), is a promising high efficient UAV configuration that can be adopted in high-density urban areas. The main object of this thesis is to improve the hovering and transition performance of tail-sitter UAV system, by studying the key technologies including modeling, model predictive control, and optimal transition trajectory. First of all, the quad-rotor tail-sitter UAV prototype 'PolyU Plus Tail-sitter' (PPT) is designed and built according to the requirement of urban cargo delivery missions. The 'plus' flying-wing configuration is selected due to the structural simplicity, high aerodynamic efficiency, and actuator reliability. Digital model of the UAV is built up for control simulation and performance analysis. The developed simulation model can reflect the main dynamic characteristics of the UAV and is able to be used as real-time simulation platform. Then, the cascaded two-loop model predictive controller (MPC) for hover flight is developed based on linearized state-space model. The measured and unmeasured disturbance are introduced into the augmented dynamic model to improve the tracking and disturbance rejection performance. The controllers are tuned and verified in the hardware-in-loop (HIL) simulation environment and then implemented to an onboard flight computer for real-time execution in the indoor VICON environment. The simulation and experimental results indicates that the proposed MPC controller has good tracking and position holding capability against disturbance such as prevailing and gusty winds. Finally, both the forward and back transition trajectories are optimized according to the minimized energy costs. The simplified 3-DOF longitudinal model is used as the dynamic constraint. The transition optimization problem is transcript into a non-linear programming (NLP) problem and solved by collocation methods. Simulation with Gazebo simulator and outdoor flight experiments are carried out with the optimized transition trajectories. The simulation and experimental results show that the optimized transition strategy enable both the forward and back transitions with less time, less height change, and energy consumption compared with traditional linear transition method.

## Video

<iframe width="560" height="315" src="https://www.youtube.com/embed/kYevywwGgjQ" frameborder="0" allowfullscreen></iframe>
