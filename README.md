<div align="center">

<img src="docs/banner.png" alt="LimX Luna humanoid robot description" width="100%">

**English** | [中文](README_cn.md)

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![ROS](https://img.shields.io/badge/ROS-1%20%7C%202-22314E?logo=ros)](#ros-1--2)
[![MuJoCo](https://img.shields.io/badge/MuJoCo-MJCF-0A7BBB)](#mujoco)
[![Isaac Lab](https://img.shields.io/badge/Isaac%20Sim%20%7C%20Lab-USD-76B900?logo=nvidia)](#isaac-sim--isaac-lab)

</div>

URDF, MuJoCo MJCF & USD models for the LimX Dynamics Luna humanoid robot (HU_L04). Provides simulation-ready robot descriptions for ROS, MuJoCo, and NVIDIA Isaac Sim / Isaac Lab.

## Highlights

- **Full-body humanoid, 27 actuated joints.** Legs, 3-DoF waist, 2-DoF head and arms, with joint specs (rotor inertia, gear ratio) in the SRDF.
- **Parallel linkages modeled faithfully.** The ankles and waist are driven through parallel linkages; the MJCF expresses them with `equality` constraints and `usd_parallel/` as true closed loops.
- **One robot, every simulator.** URDF for ROS, MJCF for MuJoCo, USD for Isaac Sim / Isaac Lab, all from the same geometry.
- **Ready for learning pipelines.** The MJCF ships actuators plus joint position/velocity and IMU sensors, and a kinematics-only variant for motion retargeting.

## Try it in 30 seconds

No ROS needed:

```bash
git clone https://github.com/limxdynamics/luna-description.git
cd luna-description
pip install mujoco
python -m mujoco.viewer --mjcf=HU_L04_description/xml/HU_L04_01.xml
```

## Available models

| | Model | Description | URDF | MuJoCo MJCF | USD |
|:-:| ----- | ----------- | ---- | ----------- | --- |
| <img src="docs/model_hu_l04.png" width="140"> | **HU_L04** | Luna humanoid, 27 actuated joints | `urdf/HU_L04_01.urdf` | `xml/HU_L04_01.xml` | `usd/HU_L04_01.usd` |
| <img src="docs/model_hu_l04_parallel.png" width="140"> | **HU_L04 (parallel)** | Same robot with the ankle and waist parallel linkages modeled as closed loops | — | — | `usd_parallel/HU_L04_01.usd` |

<sub>Second row: ankle parallel linkages (blue) rendered from the MJCF with the shank shell made translucent.</sub>

> **Note:** Luna's ankles and waist are driven through parallel linkages. URDF cannot represent closed kinematic loops, so the URDF and the matching `usd/` model use a serial approximation in which the ankle and waist joints are driven directly. The MJCF models the linkages with `equality` constraints, and `usd_parallel/` models them as true closed loops for Isaac Lab.

### Joint layout

| Group | DoF | Joints |
| ----- | :-: | ------ |
| Legs  | 6 × 2 | `hip_pitch`, `hip_roll`, `hip_yaw`, `knee`, `ankle_pitch`, `ankle_roll` |
| Waist | 3 | `waist_yaw`, `waist_roll`, `waist_pitch` |
| Head  | 2 | `head_yaw`, `head_pitch` |
| Arms  | 5 × 2 | `shoulder_pitch`, `shoulder_roll`, `shoulder_yaw`, `elbow`, `wrist_yaw` |
| **Total** | **27** | Leg and arm joints are prefixed `left_` / `right_`, e.g. `left_knee_joint`. |

### MJCF contents

| File | Actuators | Sensors | Parallel linkages |
| ---- | :-------: | ------- | ----------------- |
| `HU_L04_01.xml` | 27 | Joint position and velocity for all 27 joints, base IMU (orientation, gyro, accelerometer) | 6 `equality` constraints (left ankle, right ankle, waist) |
| `HU_L04_01_vis.xml` | — | — | — (kinematics only, for motion retargeting) |

## Directory structure

`HU_L04_description/` contains:

| Directory        | Contents                                                                                      |
| ---------------- | --------------------------------------------------------------------------------------------- |
| `urdf/`          | URDF + SRDF (joint specs: rotor inertia, gear ratio)                                          |
| `xml/`           | MuJoCo MJCF XML: `HU_L04_01.xml` (full dynamics: parallel-linkage constraints, actuators, sensors) and `HU_L04_01_vis.xml` (kinematics only, used for motion retargeting) |
| `usd/`           | NVIDIA USD files, serial approximation consistent with the URDF, + `configuration/` subdirectory |
| `usd_parallel/`  | NVIDIA USD files with the parallel linkages modeled as closed loops, + `configuration/` subdirectory |
| `meshes/`        | STL mesh files                                                                                |
| `world/`         | Simulation world files                                                                        |
| `CMakeLists.txt` | ROS package build file                                                                        |
| `package.xml`    | ROS package manifest                                                                          |

## Usage

### ROS 1 / 2

The description builds as the ROS package **`HU_L04_description`** on both ROS 1 (catkin) and ROS 2 (ament).

```bash
cd <workspace>/src
git clone https://github.com/limxdynamics/luna-description.git
cd ..
catkin_make  # or colcon build
```

The URDF references meshes as `package://HU_L04_description/meshes/...`. Outside ROS, point your loader's package path at the repository root, or use the MJCF / USD files directly.

### MuJoCo

```bash
python -m mujoco.viewer --mjcf=HU_L04_description/xml/HU_L04_01.xml
```

`HU_L04_01_vis.xml` is a kinematics-only variant (no parallel linkages, actuators or sensors) intended for motion retargeting.

### Isaac Sim / Isaac Lab

Import the USD file from `HU_L04_description/usd/HU_L04_01.usd` via Isaac Sim's reference assembly. Use `HU_L04_description/usd_parallel/HU_L04_01.usd` instead if you need the closed-loop parallel linkages.

## Related repositories

| Repository                                                         | Description                                          |
| ------------------------------------------------------------------ | ---------------------------------------------------- |
| [luna-beyondmimic](https://github.com/limxdynamics/luna-beyondmimic) | BeyondMimic motion tracking training for Luna (Isaac Lab) |

## License

Apache License 2.0 — see [LICENSE](LICENSE) and [NOTICE](NOTICE).
