# [OceanSim](https://amine123max.github.io/OceanSim_Web/)

[English](README.md) | [中文](README.zh-CN.md)

![godot](https://img.shields.io/badge/Godot-4.5-478CBF?style=flat-square&logo=godotengine&logoColor=white) ![python](https://img.shields.io/badge/Python-SDK-3776AB?style=flat-square&logo=python&logoColor=white) ![auv](https://img.shields.io/badge/AUV-Simulation-0F766E?style=flat-square) ![tcp](https://img.shields.io/badge/TCP-Protocol-1D4ED8?style=flat-square) ![license](https://img.shields.io/badge/License-MIT-166534?style=flat-square)

![OceanSim](OceanSim_Title.png)

OceanSim 是一套基于 Godot 构建的水下机器人仿真环境，面向 AUV 感知、控制、建图与数据集生成任务。项目提供可控制的 AUV 运行时、可配置海洋场景、机载传感器仿真、外部 TCP 控制接口，以及用于自动化实验和数据处理流程的 Python SDK。

OceanSim 的目标是为水下机器人研发提供一个迭代速度快、接口透明、便于扩展的实验平台，可用于控制原型验证、导航算法评估、感知管线测试、点云/RGB-D 数据生成，以及可复现实验回放。

OceanSim 当前定位不是替代认证级水动力求解器或工业级高保真仿真器，而是提供一个更易使用、可脚本化、实时运行的研究型平台，用于算法开发、数据生成和系统集成验证。

## 设计目标

- 提供可直接从 Godot 工程运行的轻量级水下机器人仿真器。
- 将仿真行为、场景配置、运行资产和 Python 工具链分层组织，方便审查和维护。
- 通过稳定的 TCP 协议向外部规划器、控制器和实验脚本暴露状态与控制接口。
- 支持面向感知、建图和机器人实验的数据导出与离线回放流程。
- 保持适合 GitHub 发布和后续协作的清晰仓库结构。

## 文档站

可发布的项目文档站单独维护在：

```text
C:\Users\Amine\Desktop\OceanSim\OceanSim_io
```

直接打开 `OceanSim_io/index.html` 可以查看生成后的手册，也可以重新构建：

```powershell
cd C:\Users\Amine\Desktop\OceanSim\OceanSim_io
python -m pip install -r requirements.txt
node build-docs.js
```

文档站使用 Sphinx 和 `sphinx_rtd_theme`。正文内容在 `OceanSim_io/source/` 下维护，发布版 HTML 会镜像到 `OceanSim_io` 根目录，方便静态托管。

## 系统架构

OceanSim 主要由六个层次组成：

- 仿真运行层：Godot 场景和脚本负责世界、AUV 本体、环境、UI、传感器、Agent、障碍物、水体物理和仿真控制。
- 车辆配置层：车辆 profile 描述默认 AUV 的物理参数、推进器布局、actuator bounds、rate limit、deadband、failure mode、Fossen 风格 6 自由度矩阵和传感器外参。
- 场景库层：JSON 场景文件定义地图尺寸、地形风格、障碍物配置、benchmark seed、语义标签和推荐车辆 profile。
- 网络接口层：基于换行分隔 JSON 的 TCP 协议允许外部程序读取状态、推进仿真、下发命令并管理障碍物或 Agent。
- Python SDK 层：`oceansim_client` 包提供客户端接口、坐标转换、记录/回放工具、点云处理、相机数据转换、适配器和示例程序。
- 资产层：模型、图片、字体、UI 图标和地形数据统一放在 `assets/` 下，避免运行资源和源代码混放。

## 核心能力

### 仿真与环境

- 基于 Godot 4.5 的实时水下仿真。
- 支持静态地图与动态地图配置。
- 支持程序化地形和面向障碍物实验的场景定义。
- 支持海流控制，用于扰动响应和鲁棒性测试。
- 支持多 Agent，用于动态交互实验。

### 车辆与传感器

- AUV 场景支持车辆 profile、actuator 约束和 Fossen 风格 6 自由度动力学 metadata。
- 包含浮力、推进器、水体物理和海流组件。
- 传感器模块覆盖相机、RGB-D、深度计、IMU、DVL、2D 激光雷达、3D 激光雷达、成像声呐、多波束声呐和侧扫声呐。
- 传感器 payload 采用结构化 schema，便于后续处理和数据集导出。

### 外部控制 API

- 内置 TCP server，可接入外部控制器、脚本和研究管线。
- 协议版本为 `2`，支持 request ID 和结构化响应 envelope。
- 核心 action 包括 `get_state`、`get_sensors`、`step`、`set_thrust`、`set_force_world`、`set_force_body`、`set_body_wrench`、`set_body_velocity`、`reset`、`pause`、`resume`、`set_time_scale`、`spawn_obstacle`、`remove_obstacle`、`set_current` 以及 Agent 管理命令。
- 支持批量命令，便于组织同步实验步骤。

### Python 研究工具链

- Python SDK 位于 `python/oceansim_client`。
- 支持通过 `pip install -e python` 进行 editable 安装。
- 提供点云转换、过滤、体素降采样和 organized range image 工具。
- 提供 RGB-D 建图工具和相机 payload 转换工具。
- 提供数据记录与离线回放工具，便于实验复查。
- 提供 ROS 风格坐标转换和适配器工具。
- 提供 Gymnasium 风格示例环境，用于强化学习实验。

## 项目结构

```text
oceansim/
├── README.md
├── README.zh-CN.md
├── LICENSE
├── project.godot
├── assets/
│   ├── data/terrain/        # 地形数据文件
│   ├── fonts/               # UI 与绘图字体
│   ├── images/branding/     # 项目图标与启动图
│   ├── models/              # AUV/USV 模型资产
│   └── ui/                  # UI 图标
├── docs/
│   └── figures/             # 文档图像和导出说明
├── python/
│   ├── oceansim_client/     # Python SDK 包
│   ├── examples/            # 控制、建图、记录与 RL 示例
│   └── tools/               # Python smoke tests 与导出工具
├── scenarios/               # 场景配置 JSON
├── scenes/                  # Godot 场景
├── scripts/                 # Godot 仿真、UI、网络和传感器脚本
├── shaders/                 # 水体与渲染 shader
├── tools/                   # Godot smoke tests 与项目工具
└── vehicle_profiles/        # AUV 车辆 profile
```

## 环境要求

- Godot 4.5 或更新版本。
- Python 3.8 或更新版本。
- Python 依赖见 `python/pyproject.toml`。

可选强化学习示例需要安装 `python[all]` 额外依赖。

## 运行仿真器

打开 Godot 工程：

```powershell
cd C:\path\to\oceansim
godot --path .
```

当前配置的入口场景为：

```text
res://scenes/FrontView.tscn
```

也可以在 Godot 编辑器中打开 `project.godot`，然后直接运行项目。

## Python SDK 安装

以 editable 模式安装 SDK：

```powershell
cd C:\path\to\oceansim
pip install -e python
```

安装可选依赖：

```powershell
pip install -e "python[all]"
```

基础连接示例：

```python
from oceansim_client import AUV

with AUV() as auv:
    state = auv.get_state()
    print(state.position, state.depth)
    auv.set_force_world(fx=0.0, fy=120.0, fz=0.0)
    auv.set_force_body(fx=0.0, fy=40.0, fz=0.0)
```

Typed agent 示例：

```python
from oceansim_client import AUV, AgentDefinition, AgentFactory

with AUV() as auv:
    target = AgentFactory.build_agent(
        auv,
        AgentDefinition(
            agent_name="target_01",
            agent_type="proxy_auv",
            is_main_agent=False,
            location=[4.0, -7.0, 6.0],
            control_scheme="external_velocity",
            metadata={"role": "moving_target"},
        ),
    )
    target.act([0.2, 0.0, -0.1])
```

## 场景库

场景文件位于 `scenarios/`。每个场景描述地图尺寸、地形生成方式、障碍物配置、benchmark seed、语义标签、验收提示和推荐车辆 profile。

当前包含的场景 profile：

- `Static_Map.json`
- `Dynamic_Map.json`
- `NarrowMap.json`
- `PaleMap.json`

## 许可证

OceanSim 使用 MIT License 发布，详情见 `LICENSE`。
