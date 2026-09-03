# 中文 | [English](README.md)

# LimX Luna 机器人描述

LimX Dynamics Luna 人形机器人（HU_L04）的 URDF、MuJoCo MJCF 和 USD 模型文件。提供仿真就绪的机器人描述，支持 ROS、MuJoCo 和 NVIDIA Isaac Sim / Isaac Lab。

## 可用型号

| 型号 | 描述 | URDF | MuJoCo MJCF | USD |
| ---- | ---- | ---- | ----------- | --- |
| **HU_L04** | Luna 人形，27 个主动关节 | `urdf/HU_L04_01.urdf` | `xml/HU_L04_01.xml` | `usd/HU_L04_01.usd` |
| **HU_L04（并联）** | 同一机器人，脚踝和腰部的并联连杆按真实闭环建模 | — | — | `usd_parallel/HU_L04_01.usd` |

> **注意：** Luna 的脚踝和腰部通过并联连杆驱动。URDF 无法表达闭环运动链，因此 URDF 及与之对应的 `usd/` 采用串联近似，把脚踝和腰部关节视为直接驱动。MJCF 用 `equality` 约束表达并联连杆；`usd_parallel/` 则把连杆建成真实闭环，供 Isaac Lab 使用。

## 目录结构

`HU_L04_description/` 包含：

| 目录 | 内容 |
|------|------|
| `urdf/` | URDF + SRDF（关节参数：转子惯量、减速比） |
| `xml/` | MuJoCo MJCF XML（含并联连杆约束、执行器和传感器） |
| `usd/` | NVIDIA USD 文件，与 URDF 一致的串联近似，+ `configuration/` 子目录 |
| `usd_parallel/` | NVIDIA USD 文件，并联连杆按闭环建模，+ `configuration/` 子目录 |
| `meshes/` | STL 网格文件 |
| `world/` | 仿真世界文件 |
| `CMakeLists.txt` | ROS 包构建文件 |
| `package.xml` | ROS 包清单 |

## 快速开始

### ROS 1/2

```bash
cd <workspace>/src
git clone https://github.com/limx-luna/luna-description.git
cd ..
catkin_make  # 或 colcon build
```

### MuJoCo

```bash
python -m mujoco.viewer --mjcf=HU_L04_description/xml/HU_L04_01.xml
```

### Isaac Sim / Isaac Lab

通过 Isaac Sim 的参考装配功能导入 `HU_L04_description/usd/HU_L04_01.usd`。若需要闭环并联连杆，改用 `HU_L04_description/usd_parallel/HU_L04_01.usd`。

## 许可证

Apache License 2.0 — 详见 [LICENSE](LICENSE)。

## 相关仓库

| 仓库 | 描述 |
| ---- | ---- |
| [luna-beyondmimic](https://github.com/limx-luna/luna-beyondmimic) | Luna 的 BeyondMimic 动作跟踪训练（Isaac Lab） |

> 如果这些模型对你有帮助，欢迎给仓库点个 Star。
