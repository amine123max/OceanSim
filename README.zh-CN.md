# [OceanSim](https://amine123max.github.io/OceanSim_Web/)

[English](README.md) | [中文](README.zh-CN.md) | [Documentation](https://amine123max.github.io/OceanSim_Web/)

![godot](https://img.shields.io/badge/Godot-4.5-478CBF?style=flat-square&logo=godotengine&logoColor=white) ![python](https://img.shields.io/badge/Python-SDK-3776AB?style=flat-square&logo=python&logoColor=white) ![auv](https://img.shields.io/badge/AUV-Simulation-0F766E?style=flat-square) ![tcp](https://img.shields.io/badge/TCP-Protocol-1D4ED8?style=flat-square) ![license](https://img.shields.io/badge/License-MIT-166534?style=flat-square)

![OceanSim](OceanSim_Title.png)

OceanSim 是一套面向 AUV、USV 与 UAV 自主系统研究的实时机器人仿真平台，重点服务海洋相关环境中的感知开发、导航评估和数据集生成。项目基于 Godot 构建，提供可配置海洋场景，以及覆盖水下、水面和空中域的可控载体行为、机载传感器仿真和外部控制接口。

OceanSim 注重快速研究迭代：足够轻量，便于原型验证；足够结构化，便于可复现实验；同时具备扩展性，可支持异构机器人平台上的控制、建图、感知、规划和多 Agent 交互研究。

## Video_demo

![OceanSim video demo](OceanSim.gif)

该演示展示四类代表性地图场景：

- `Narrow_Map`：面向狭窄海洋通道的受限导航测试。
- `Pale_Map`：面向稀疏、低对比地形的感知与建图鲁棒性测试。
- `Static_Map`：面向固定障碍场景的可重复规划基准评估。
- `Dynamic_Map`：面向动态障碍和多 Agent 交互的实验场景。

## 核心能力

- 支持面向海洋环境的实时仿真，可配置地形、障碍物、海流扰动和场景参数。
- 支持 AUV、USV 与 UAV 多类型载体运行时，用于测试载体行为、导航逻辑和闭环控制策略。
- 面向传感器的仿真能力，覆盖相机、RGB-D、深度计、IMU、DVL、激光雷达、声呐和点云工作流。
- 通过结构化 TCP 协议接入外部规划器、控制器、学习智能体和研究脚本。
- 支持基于 Python 的自动化实验、数据采集、回放、感知处理和评估流程。

## 研究应用

- AUV、USV 与 UAV 自主控制和轨迹验证。
- 静态和动态环境下的路径规划与避障评估。
- 覆盖水下、水面和空中视角的海洋感知、点云建图、RGB-D 处理和多传感器融合实验。
- 面向机器人、学习感知和可复现 benchmark 的数据集生成。
- 面向协同或对抗任务的多 Agent 海洋仿真研究。

## 文档

完整在线文档发布在 [OceanSim_Web](https://amine123max.github.io/OceanSim_Web/)，包含仿真器使用、场景配置、Python SDK 工作流、TCP API 接入说明，以及面向研究实验的参考资料。

## 项目状态

OceanSim 是面向 AUV、USV 与 UAV 算法开发、实验评估和数据生成的研究型仿真平台，不用于替代认证级水动力、空气动力或工业级高保真仿真套件。

## 许可证

OceanSim 使用 MIT License 发布。
