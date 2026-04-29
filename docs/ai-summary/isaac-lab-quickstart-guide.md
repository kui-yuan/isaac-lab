# Isaac Lab Quickstart Guide

This guide introduces the essential steps and concepts needed to start using Isaac Lab, including installation, running reinforcement learning (RL), exploring environments, and creating new projects.

---

## Core Concepts

Isaac Lab is built around two key ideas:

### 1. Vectorization
- Enables parallel execution of many environment instances.
- Speeds up RL training by collecting data across multiple environments simultaneously.
- Core mechanism for efficient large-scale training.

### 2. Modular Design
- System components are interchangeable and extensible.
- Supports abstraction layers (e.g., `ActionManager`) to make environments reusable across robots or tasks.
- Encourages flexible and scalable project design.

---

## Quick Installation Guide

### 1. Create Virtual Environment

**Using conda**
```bash
conda create -n env_isaaclab python=3.11
conda activate env_isaaclab
````

**Using uv (Linux)**

```bash
uv venv --python 3.11 --seed env_isaaclab
source env_isaaclab/bin/activate
```

**Using uv (Windows)**

```bash
uv venv --python 3.11 env_isaaclab
env_isaaclab\Scripts\activate
```

---

### 2. Install PyTorch (CUDA-enabled)

```bash
pip install -U torch==2.7.0 torchvision==0.22.0 \
  --index-url https://download.pytorch.org/whl/cu128
```

---

### 3. Update pip

```bash
pip install --upgrade pip
```

---

### 4. Install Isaac Sim

```bash
pip install "isaacsim[all,extscache]==5.1.0" \
  --extra-index-url https://pypi.nvidia.com
```

---

### 4.5. Install CMake

This avoids the requirement of sudo.

```bash
conda install -y cmake=3.26 make gcc gxx
```

---

### 5. Install Isaac Lab

```bash
git clone https://github.com/isaac-sim/IsaacLab.git
cd IsaacLab
./isaaclab.sh --install   # Linux
isaaclab.bat --install    # Windows
```

---

## Quick Start via Cloud (Isaac Launchable)

* Run Isaac Lab directly in the browser.
* Provides:

  * VS Code for development
  * Streamed Isaac Sim UI
* Built on NVIDIA Brev (pay-as-you-go cloud compute).
* No local installation required.

---

## Launch Training

Training scripts are located in:

```
isaaclab/scripts/reinforcement_learning/
```

Example:

```bash
python scripts/reinforcement_learning/skrl/train.py --task=Isaac-Ant-v0
```

Useful options:

* `--num_envs`: number of parallel environments
* `--headless`: run without GUI

CLI arguments override config defaults.

---

## List Available Environments

```bash
python scripts/environments/list_envs.py
```

Example entries:

| Task Name           | Entry Point                              |
| ------------------- | ---------------------------------------- |
| Isaac-Ant-Direct-v0 | isaaclab_tasks.direct.ant.ant_env:AntEnv |
| Isaac-Ant-v0        | isaaclab.envs:ManagerBasedRLEnv          |

### Two workflows:

* **Direct**: simpler, faster setup
* **Manager-based**: modular, scalable design

---

## Generate a New Project

```bash
./isaaclab.sh --new
```

Options:

* External vs Internal project
* Direct vs Manager workflow
* RL frameworks (multiple allowed)

Install project:

```bash
pip install -e source/<project-name>
```

---

## Environment Registration Example

```python
import gymnasium as gym

gym.register(
    id="Template-isaaclabtutorial_env-v0",
    entry_point="module.env:EnvClass",
    disable_env_checker=True,
    kwargs={
        "env_cfg_entry_point": "module.env_cfg:EnvCfg",
        "skrl_cfg_entry_point": "agents.ppo_cfg:PPORunnerCfg",
    },
)
```

* `entry_point`: Python path to environment class
* Required for Gymnasium integration

---

## Configurations (`@configclass`)

Configuration classes:

* Use `@configclass` decorator
* No `__init__` method
* Define environment parameters declaratively

Example:

```python
@configclass
class CartpoleEnvCfg(DirectRLEnvCfg):
    episode_length_s = 5.0
    action_scale = 100.0

    sim = SimulationCfg(dt=1/120)

    scene = InteractiveSceneCfg(
        num_envs=4096,
        env_spacing=4.0
    )
```

### Key Points:

* Used for vectorization control
* Enable easy parameter overrides via CLI
* Provide structured access to all environment settings

---

## Apps and Simulation (Standalone Workflow)

To use Isaac Sim directly:

```python
from isaaclab.app import AppLauncher

app_launcher = AppLauncher(args_cli)
simulation_app = app_launcher.app
```

### Notes:

* `AppLauncher` initializes the simulation environment
* Required before importing many Isaac modules
* Used for:

  * Simulation lifecycle control
  * GUI and extension management

---

## Summary Workflow

1. Create virtual environment
2. Install PyTorch + Isaac Sim + Isaac Lab
3. Launch training script
4. Explore environments
5. Generate custom project
6. Modify configs and extend environments

---
