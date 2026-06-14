# [OceanSim](https://amine123max.github.io/OceanSim_Web/)

[English](README.md) | [中文](README.zh-CN.md)

![godot](https://img.shields.io/badge/Godot-4.5-478CBF?style=flat-square&logo=godotengine&logoColor=white) ![python](https://img.shields.io/badge/Python-SDK-3776AB?style=flat-square&logo=python&logoColor=white) ![auv](https://img.shields.io/badge/AUV-Simulation-0F766E?style=flat-square) ![tcp](https://img.shields.io/badge/TCP-Protocol-1D4ED8?style=flat-square) ![license](https://img.shields.io/badge/License-MIT-166534?style=flat-square)

![OceanSim](OceanSim_Title.png)

OceanSim is a research-oriented underwater robotics simulation environment built on Godot. It provides a controllable AUV runtime, configurable marine scenarios, onboard sensor simulation, an external TCP control interface, and a Python SDK for automated experiments and dataset workflows.

The project is designed for fast iteration in underwater robotics research and development: control prototyping, navigation evaluation, perception pipeline testing, point-cloud/RGB-D data generation, and reproducible experiment playback. OceanSim prioritizes transparent interfaces, scriptable workflows, and a clean project structure suitable for open-source publication.

OceanSim is not intended to replace certification-grade hydrodynamic solvers or high-fidelity industrial simulators. Its current focus is an accessible, extensible, real-time research platform for algorithm development and data generation.

## Design Goals

- Provide a lightweight underwater robotics simulator that can run directly from a Godot project.
- Keep simulation behavior, scenarios, assets, and Python tooling organized in separate, auditable layers.
- Expose simulator state and controls through a stable TCP protocol for external planners and controllers.
- Support dataset export and replay workflows for perception, mapping, and robotics experiments.
- Maintain a repository layout that is clean enough for GitHub publication and future collaboration.

## Documentation

The publishable documentation site is maintained separately in:

```text
C:\Users\Amine\Desktop\OceanSim\OceanSim_io
```

Open `OceanSim_io/index.html` for the generated manual, or rebuild it with:

```powershell
cd C:\Users\Amine\Desktop\OceanSim\OceanSim_io
python -m pip install -r requirements.txt
node build-docs.js
```

The documentation site uses Sphinx with `sphinx_rtd_theme`. Content is edited under `OceanSim_io/source/`, while release-style HTML is mirrored to the `OceanSim_io` root for static hosting.

## System Architecture

OceanSim is organized into six main layers:

- Simulation runtime: Godot scenes and scripts implement the world, AUV body, environment, UI, sensors, agents, obstacles, water physics, and simulation controls.
- Vehicle configuration: vehicle profiles describe the default AUV setup, including physical parameters, thruster layout, actuator bounds, rate limits, deadbands, failure modes, Fossen-style 6-DOF matrices, and sensor extrinsics.
- Scenario library: JSON scenario files define map size, terrain style, obstacle profile, benchmark seeds, semantic tags, and recommended vehicle profiles.
- Networking layer: a line-delimited JSON TCP protocol allows external programs to query state, step the simulator, issue commands, and manage obstacles or agents.
- Python SDK: the `oceansim_client` package provides a typed client interface, coordinate conversions, recording/replay utilities, point-cloud helpers, camera payload conversion, adapters, and examples.
- Asset layer: models, images, fonts, UI icons, and terrain data are grouped under `assets/` to keep runtime resources separate from source code.

## Key Capabilities

### Simulation And Environment

- Real-time Godot 4.5 underwater simulation.
- Configurable static and dynamic maps.
- Procedural terrain and obstacle-oriented scenario definitions.
- Ocean-current controls for disturbance and robustness testing.
- Multi-agent support for dynamic interaction experiments.

### Vehicle And Sensors

- AUV scene with vehicle profile support, actuator constraints, and Fossen-style six-degree dynamics metadata.
- Buoyancy, thruster, water-physics, and ocean-current components.
- Sensor modules for camera, RGB-D, depth, IMU, DVL, 2D lidar, 3D lidar, imaging sonar, multibeam sonar, and side-scan sonar.
- Sensor payload schemas intended for downstream processing and dataset export.

### External Control API

- TCP server for external controllers, scripts, and research pipelines.
- Protocol version `2` with request IDs and structured response envelopes.
- Core actions include `get_state`, `get_sensors`, `step`, `set_thrust`, `set_force_world`, `set_force_body`, `set_body_wrench`, `set_body_velocity`, `reset`, `pause`, `resume`, `set_time_scale`, `spawn_obstacle`, `remove_obstacle`, `set_current`, and agent-management commands.
- Batch command support for coordinated experiment steps.

### Python Research Toolkit

- Python SDK package under `python/oceansim_client`.
- Editable installation through `pip install -e python`.
- Point-cloud conversion, filtering, voxel downsampling, and organized range-image helpers.
- RGB-D mapping utilities and camera payload conversion.
- Recording and replay utilities for offline inspection.
- Robotics coordinate conversion helpers and adapter utilities.
- Gymnasium-style example environment for reinforcement-learning experiments.

## Repository Layout

```text
oceansim/
├── README.md
├── README.zh-CN.md
├── LICENSE
├── project.godot
├── assets/
│   ├── data/terrain/        # Terrain data files
│   ├── fonts/               # UI and plotting fonts
│   ├── images/branding/     # Project icon and splash images
│   ├── models/              # AUV/USV model assets
│   └── ui/                  # UI icons
├── docs/
│   └── figures/             # Documentation figures and exported references
├── python/
│   ├── oceansim_client/     # Python SDK package
│   ├── examples/            # Control, mapping, recording, and RL examples
│   └── tools/               # Python smoke tests and export utilities
├── scenarios/               # Scenario configuration JSON files
├── scenes/                  # Godot scenes
├── scripts/                 # Godot simulation, UI, networking, and sensor scripts
├── shaders/                 # Water and rendering shaders
├── tools/                   # Godot smoke tests and project utilities
└── vehicle_profiles/        # AUV vehicle profile definitions
```

## Requirements

- Godot 4.5 or newer.
- Python 3.8 or newer.
- Python dependencies declared in `python/pyproject.toml`.

Optional reinforcement-learning examples require the `python[all]` extra.

## Running The Simulator

Open the Godot project:

```powershell
cd C:\path\to\oceansim
godot --path .
```

The configured entry scene is:

```text
res://scenes/FrontView.tscn
```

From the Godot editor, open `project.godot` and run the project normally.

## Python SDK Installation

Install the SDK in editable mode:

```powershell
cd C:\path\to\oceansim
pip install -e python
```

Install optional dependencies:

```powershell
pip install -e "python[all]"
```

Basic connection example:

```python
from oceansim_client import AUV

with AUV() as auv:
    state = auv.get_state()
    print(state.position, state.depth)
    auv.set_force_world(fx=0.0, fy=120.0, fz=0.0)
    auv.set_force_body(fx=0.0, fy=40.0, fz=0.0)
```

Typed agent example:

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

## Scenario Library

Scenario files are stored in `scenarios/`. Each scenario describes map dimensions, terrain generation, obstacle configuration, benchmark seeds, semantic tags, acceptance hints, and the recommended vehicle profile.

Included scenario profiles:

- `Static_Map.json`
- `Dynamic_Map.json`
- `NarrowMap.json`
- `PaleMap.json`

## License

OceanSim is released under the MIT License. See `LICENSE` for details.
