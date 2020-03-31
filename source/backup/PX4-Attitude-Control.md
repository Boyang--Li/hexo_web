---
title: PX4 Attitude Control
date: 2017-04-12 16:48:52
tags: PX4
categories: reading
---

Today, I am going to deeply analysis the multi-rotor attitude control code in PX4. 
On one hand, I need to know the full process about the flight controller.

# Files List

路径：Firmware/src/modules/mc_att_control/

文件：1. CMakeLists.txt, 2. mc_att_control_main.cpp, 3. mc_att_control_params.c
<!-- more -->

## CMakeLists.txt （给Cmake提供信息？）

```bash
px4_add_module(
    MODULE modules__mc_att_control
    MAIN mc_att_control
    STACK_MAIN 1200
    STACK_MAX 3500
    COMPILE_FLAGS
    SRCS
        mc_att_control_main.cpp
    DEPENDS
        platforms__common
    )
```

## mc_att_control_params.c （姿态控制所需要的所有参数）

| Axis          | Parameter Name       |Example|
| --------------|--------------------------|-------|
| Roll/Pitch    | time constant        |(MC_ROLL_TC, 0.2f)|
| Roll/Pitch/Yaw| P gain               |(MC_ROLL_P, 6.5f)|
| Roll/Pitch/Yaw| rate P gain          |(MC_ROLLRATE_P, 0.15f)|
| Roll/Pitch/Yaw| rate I gain          |(MC_ROLLRATE_I, 0.05f)|
| Roll/Pitch/Yaw| rate integrator limit|(MC_RR_INT_LIM, 0.30f)|
| Roll/Pitch/Yaw| rate D gain          |(MC_ROLLRATE_D, 0.003f)|
| Roll/Pitch/Yaw| rate feedforward     |(MC_ROLLRATE_FF, 0.0f)|
| Yaw           |feed forward          |(MC_YAW_FF, 0.5f)
| -             |    -                 |-|
| Roll/Pitch/Yaw| Max rate in automous mode|(MC_ROLLRATE_MAX, 220.0/220/200f)
|-                          | -                              | -                      |
|Battery power level scaler | boolean                        |(MC_BAT_SCALE_EN, 0)    |
|-                          |-                               | -                      |
| TPA P/I/D Breakpoint      | Throttle PID Attenuation (TPA) | (MC_TPA_BREAK_P, 1.0f) |
| TPA Rate P/I/D  | Throttle PID Attenuation (TPA)  |  (MC_TPA_RATE_P, 0.0f)|

## mc_att_control_main.cpp （姿态控制控制率）

### Key Points

- 控制率主要有两环，姿态（角度）误差用P控制，角速率误差用PD控制
- Yaw响应比Roll/Pitch慢
- 对于小误差控制器控制旋翼机以最短推力矢量方向旋转，再独立旋转Yaw,因此实际旋转轴并不固定
- 对于大误差控制器使旋翼机沿固定轴旋转
- 这两种方式根据角度误差的比重无缝融合
- 当推力矢量方向接近水平（pi/2）时，考虑到奇点Yaw目标会失效
- 控制器不依赖欧拉角，欧拉角生成只是为了便于理解和记录
- 当旋转矩阵目标点无效时，会从欧拉角产生，这样可以和老的位置控制器兼容

### #include<>

- drivers
- lib
- px4
- systemlib
- uORB

### App definition

* Multicopter attitude control app start / stop handling function
`
    extern "C" __EXPORT int mc_att_control_main(int argc, char *argv[]);
`
<u>(extern "C" {}: c,c++ 混合编程时的一个处理方法）</u>

### #define
又定义了一些似乎重复和新的变量

### class MulticopterAttitudeControl 
public:

    MulticopterAttitudeControl(); 建立
    ~MulticopterAttitudeControl();销毁
    int		start();开始
    
private:

   - 一堆int
   - 一堆struct
   - 一个Union
   - 一堆math::Vector
   - 一个大结构体 _params_handles 基本上是params 文件里定义的参数，又定义一遍
   - 同上 _params 感觉跟上面有些重复，但是类型不一样
   - 一堆int,void的函数

### namespace mc_att_control
`
namespace mc_att_control
{
    MulticopterAttitudeControl	*g_control;
}
`
### 建立MulticopterAttitudeControl::MulticopterAttitudeControl()

### 销毁MulticopterAttitudeControl::~MulticopterAttitudeControl()

### 参数更新 int MulticopterAttitudeControl::parameters_update()

### 一堆各种poll 
参数Poll,飞行器模式poll,手动poll,姿态目标点poll,rate目标点poll,arm状态poll,
飞行器状态poll,电机极限poll,电池状态poll,

### <font color=red>姿态控制 control_attitude</font>

### 油门 Throttle PID attenuation

### <font color=red>姿态rate控制 control_attitude_rates</font>

### task_main

### start

### mc_att_control_main
