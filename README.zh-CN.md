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

## 快速开始

**环境要求：** Godot 4.5+、Python 3.8+、NumPy

```bash
# 1. 运行仿真器
cd /path/to/OceanSim/oceansim
godot --path .

# 2. 安装 Python SDK（可编辑模式）
cd /path/to/OceanSim/oceansim
pip install -e python

# 3. 验证连接
python -c "
from oceansim_client import OceanSimClient
client = OceanSimClient(host='localhost', port=9876, timeout=5.0)
if client.connect():
    response = client.send_command('get_state')
    print(response['type'], response.get('status'))
    client.disconnect()
"
```

## 仓库结构

```
oceansim/
├── assets/models/          # AUV 和 USV 模型资源
├── scenes/                 # Godot 场景文件和入口
├── scripts/
│   ├── auv/                # AUV 机体、推进器、浮力
│   ├── sensors/            # 导航、视觉、测距、声呐
│   └── network/            # TCP 服务器和协议封装
├── python/oceansim_client/ # Python SDK、建图、数据集
├── vehicle_profiles/       # 载体物理配置
└── tools/                  # 冒烟测试和发布门禁
```

## 文档

完整在线文档发布在 [OceanSim_Web](https://amine123max.github.io/OceanSim_Web/)，包含仿真器使用、场景配置、Python SDK 工作流、TCP API 接入说明，以及面向研究实验的参考资料。

## 引用

如果您的研究使用了 OceanSim，请引用：

```bibtex
@software{oceansim2026,
  author       = {OceanSim Contributors},
  title        = {OceanSim: A Real-Time Robotics Simulation Platform for AUV, USV, and UAV Autonomy Research},
  year         = {2026},
  publisher    = {GitHub},
  url          = {https://github.com/amine123max/OceanSim},
  howpublished = {\url{https://github.com/amine123max/OceanSim}}
}
```

## 项目状态

OceanSim 是面向 AUV、USV 与 UAV 算法开发、实验评估和数据生成的研究型仿真平台，不用于替代认证级水动力、空气动力或工业级高保真仿真套件。

## 贡献指南

欢迎参与贡献！请阅读[贡献指南](CONTRIBUTING.md)了解开发环境配置、代码规范和 Pull Request 流程。

请注意，本项目遵循[行为准则](CODE_OF_CONDUCT.md)。参与本项目即表示您同意遵守其条款。

## 许可证

OceanSim 使用 MIT License 发布。
