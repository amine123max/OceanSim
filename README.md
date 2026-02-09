# OceanSim - 水下机器人仿真器

基于 Godot 4.5 + Python 的 3D 水下机器人（AUV）仿真平台，支持路径规划、避障、强化学习等算法验证。

## 项目结构

```
OceanSim/
├── oceansim/                    # Godot 4.5 项目（仿真引擎）
│   ├── scenes/                  # 场景文件
│   │   ├── main.tscn            # 主场景
│   │   └── auv.tscn             # AUV 场景
│   ├── scripts/                 # GDScript 脚本
│   │   ├── main.gd              # 主控制器 + TCP 命令处理
│   │   ├── camera_controller.gd # 多视窗布局管理
│   │   ├── map_manager.gd       # 地图管理
│   │   ├── auv/                 # AUV 物理模型
│   │   │   ├── auv_body.gd      # AUV 刚体 + 运动学
│   │   │   ├── buoyancy.gd      # 浮力系统
│   │   │   └── thruster.gd      # 推进器
│   │   ├── sensors/             # 传感器
│   │   │   ├── lidar_2d.gd      # 2D 激光雷达
│   │   │   ├── lidar_3d.gd      # 3D 激光雷达
│   │   │   ├── imu.gd           # IMU
│   │   │   ├── depth_sensor.gd  # 深度计
│   │   │   └── camera_sensor.gd # 摄像头
│   │   ├── environment/         # 环境
│   │   │   ├── terrain_generator.gd  # 柏林噪声峡谷地形
│   │   │   ├── water_physics.gd      # 水阻力
│   │   │   └── ocean_current.gd      # 洋流
│   │   ├── network/             # 通信
│   │   │   ├── tcp_server.gd    # TCP 服务器
│   │   │   └── protocol.gd      # JSON 协议
│   │   ├── views/               # 视角
│   │   │   ├── topdown_view.gd  # 俯视全景
│   │   │   ├── follow_view.gd   # 尾部跟随
│   │   │   └── lidar_view.gd    # LiDAR 雷达图
│   │   ├── ui/                  # UI 控件
│   │   │   ├── plot_graph.gd    # 实时折线图
│   │   │   ├── data_plotter.gd  # 数据面板
│   │   │   ├── lidar_map_view.gd    # 2D LiDAR 可视化
│   │   │   └── lidar_3d_view.gd     # 3D LiDAR 可视化
│   │   └── obstacles/           # 障碍物
│   │       └── obstacle_manager.gd
│   └── shaders/                 # 着色器
│       └── water_surface.gdshader
├── python/                      # Python SDK
│   ├── oceansim_client/
│   │   ├── __init__.py
│   │   ├── client.py            # TCP 客户端
│   │   ├── auv_interface.py     # AUV 高级 API
│   │   └── gym_env.py           # OpenAI Gym 环境
│   ├── examples/
│   │   ├── basic_control.py     # 基础控制示例
│   │   ├── pid_depth_control.py # PID 深度控制
│   │   └── rl_gym_example.py    # 强化学习示例
│   ├── pyproject.toml
│   └── requirements.txt
└── dist/                        # 导出目录（exe）
```

## 快速开始

### 1. 运行仿真器

**方式 A：Godot 编辑器**
1. 安装 [Godot 4.5](https://godotengine.org/)
2. 打开 `oceansim/project.godot`
3. 按 F5 运行

**方式 B：独立 exe**
1. 双击 `dist/OceanSim.exe`（需先导出）

### 2. 安装 Python SDK

```bash
cd python
pip install -e .
```

### 3. Python 控制

```python
from oceansim_client import AUV

auv = AUV()
auv.connect()

# 获取状态 (x=横向, y=纵向, z=上下)
state = auv.get_state()
print(state.position, state.speed, state.depth)

# 施加力
auv.set_force(fx=100, fy=0, fz=0)  # 向右移动

# 获取传感器
lidar = auv.get_lidar_2d()

# 重新生成地图
auv.generate_map()

# 重置仿真
auv.reset()
auv.disconnect()
```

### 4. Gym 强化学习

```python
from oceansim_client.gym_env import OceanSimEnv

env = OceanSimEnv(target=[5.0, 10.0, -10.0], max_steps=500)
obs, info = env.reset()

for _ in range(500):
    action = env.action_space.sample()  # [fx, fy, fz] normalized to [-1, 1]
    obs, reward, done, truncated, info = env.step(action)
    if done:
        obs, info = env.reset()

env.close()
```

## 界面说明

```
+-- 2D LiDAR --+-- 跟随视角 --+-- 数据图表 ----------+
| 红色声呐环    | AUV尾部视角  | Velocity (m/s)       |
| 绿色点云     |              | Angular velocity     |
| 蓝色轨迹     |              | Acceleration (m/s²)  |
+--------------+--------------+ Angular accel (rad/s²)|
|  主视角（俯视全景）          |                      |
|  AUV 在中心，峡谷地形        |                      |
|  黄色线框包裹 AUV            |                      |
+-----------------------------+----------------------+
  状态信息: Pos(x,y,z) | Depth | Speed | Heading
```

## 键盘控制（无 Python 连接时）

| 按键 | 功能 |
|------|------|
| W/S | 前进/后退 |
| A/D | 左移/右移 |
| Space/Shift | 上浮/下潜 |
| End | 重置仿真 |
| 鼠标滚轮 | 缩放俯视高度 |

## TCP 协议

仿真器监听 TCP 端口 **9876**，使用 JSON 协议通信（换行符分隔）。

### 命令

```json
{"type": "command", "action": "set_force", "data": {"force": [fx, fy, fz]}}
{"type": "command", "action": "get_state"}
{"type": "command", "action": "get_lidar_2d"}
{"type": "command", "action": "get_lidar_3d"}
{"type": "command", "action": "generate_map"}
{"type": "command", "action": "step", "data": {"force": [fx, fy, fz]}}
```

### 仿真控制

```json
{"type": "sim_control", "action": "reset"}
{"type": "sim_control", "action": "pause"}
{"type": "sim_control", "action": "resume"}
{"type": "sim_control", "action": "set_time_scale", "value": 2.0}
```

### 状态返回

```json
{
  "type": "state",
  "data": {
    "position": [x, y, z],
    "velocity": [vx, vy, vz],
    "depth": 10.0,
    "speed": 1.5,
    "heading_deg": 0.78,
    "acceleration": 0.5,
    "angular_velocity_yaw": 0.1,
    "angular_acceleration": 0.05,
    "sim_time": 12.3
  }
}
```

## 坐标系

| 轴 | 含义 | 正方向 |
|----|------|--------|
| x | 横向 | 右 |
| y | 纵向 | 前进方向 |
| z | 垂直 | 向上 |

## 导出为独立 exe

1. Godot 编辑器 -> Editor -> Manage Export Templates -> 下载
2. Project -> Export -> Add Preset -> Windows Desktop
3. Export Project -> 选择 `dist/OceanSim.exe`

导出后 Python 可自动启动：
```python
auv = AUV(auto_launch=True, exe_path="dist/OceanSim.exe")
auv.connect()
```

## AUV 参数

| 参数 | 值 |
|------|-----|
| 最大速度 | 3.0 m/s |
| 最大垂直速度 | 1.5 m/s |
| 质量 | 50 kg |
| 模型尺寸 | 0.5m × 0.167m × 0.083m |
| LiDAR 2D 量程 | 30m, 3° 角分辨率 |
| LiDAR 3D 量程 | 30m, 8 通道 |

## 地形

使用柏林噪声生成峡谷地形：
- 峡谷宽度：5m
- 峭壁高度：海面以上 5m 至海底 -30m
- AUV 巡航深度：-10m
- 地图面积：80m × 80m
- 每次启动随机生成路径
