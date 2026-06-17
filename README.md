# [OceanSim](https://amine123max.github.io/OceanSim_Web/)

[English](README.md) | [中文](README.zh-CN.md) | [Documentation](https://amine123max.github.io/OceanSim_Web/)

![godot](https://img.shields.io/badge/Godot-4.5-478CBF?style=flat-square&logo=godotengine&logoColor=white) ![python](https://img.shields.io/badge/Python-SDK-3776AB?style=flat-square&logo=python&logoColor=white) ![auv](https://img.shields.io/badge/AUV-Simulation-0F766E?style=flat-square) ![tcp](https://img.shields.io/badge/TCP-Protocol-1D4ED8?style=flat-square) ![license](https://img.shields.io/badge/License-MIT-166534?style=flat-square)

![OceanSim](OceanSim_Title.png)

OceanSim is a real-time underwater robotics simulation platform for AUV autonomy research, marine perception development, navigation evaluation, and dataset generation. Built on Godot, it provides configurable ocean environments, controllable vehicle behavior, simulated onboard sensors, and external interfaces for algorithm-driven experiments.

The simulator is designed for fast research iteration: lightweight enough for rapid prototyping, structured enough for reproducible evaluation, and extensible enough to support control, mapping, perception, planning, and multi-agent studies in underwater environments.

## Video_demo

![OceanSim video demo](OceanSim.gif)

The demo highlights four representative map profiles:

- `Narrow_Map`: constrained navigation through narrow underwater passages.
- `Pale_Map`: sparse, low-contrast terrain for perception and mapping robustness.
- `Static_Map`: fixed-obstacle scenarios for repeatable planning benchmarks.
- `Dynamic_Map`: moving-obstacle and multi-agent scenes for interaction experiments.

## Core Capabilities

- Real-time underwater simulation with configurable terrain, obstacles, ocean-current disturbance, and scenario parameters.
- AUV-centered runtime for testing vehicle behavior, navigation logic, and closed-loop control strategies.
- Sensor-oriented simulation for camera, RGB-D, depth, IMU, DVL, lidar, sonar, and point-cloud workflows.
- External control through a structured TCP protocol for integrating planners, controllers, learning agents, and research scripts.
- Python-based experimentation workflows for automation, data capture, replay, perception processing, and evaluation.

## Research Applications

- Autonomous underwater vehicle control and trajectory validation.
- Path planning and obstacle-avoidance evaluation under static and dynamic conditions.
- Underwater perception, point-cloud mapping, RGB-D processing, and sensor-fusion experiments.
- Dataset generation for robotics, learning-based perception, and reproducible benchmark studies.
- Multi-agent marine simulation for cooperative or adversarial interaction research.

## Documentation

Full documentation is available at [OceanSim_Web](https://amine123max.github.io/OceanSim_Web/), including simulator usage, scenario configuration, Python SDK workflows, TCP API integration, and experiment-oriented references.

## Project Status

OceanSim is a research-oriented simulator for algorithm development, evaluation, and data generation. It is not intended to replace certification-grade hydrodynamic solvers or high-fidelity industrial simulation suites.

## License

OceanSim is released under the MIT License.
