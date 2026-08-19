# IRL Control – MuJoCo Bimanual Manipulation

A MuJoCo-based simulation and control framework for **bimanual robot manipulation**. This repository provides tools for modeling, simulating, controlling, and evaluating dual-arm manipulation tasks using operational-space control, admittance control, configurable robot parameters, action sequences, and external input devices.

The project is based on the **IRL Control** framework originally developed by the Interactive Robotics Lab at Arizona State University and has been adapted and extended for the current research and experimentation workflow.

> **Repository:** https://github.com/Samriddhi-dubey06/samriddhi_irl_control

---

## Overview

Bimanual manipulation requires coordinated control of two robot arms while maintaining accurate end-effector motion and interaction with the environment.

This repository provides a structured MuJoCo environment for developing and testing such controllers. The framework separates robot models, control algorithms, configuration files, simulation scenes, action sequences, input devices, and task examples, making it suitable for research and rapid experimentation.

The framework uses **MuJoCo** as the physics simulation backend and provides interfaces for implementing low-level robot control and higher-level manipulation behaviors.

---

## Key Features

* **Bimanual robot manipulation**
* **MuJoCo-based physics simulation**
* **Operational Space Control (OSC)**
* **Admittance control**
* **External force interaction**
* **Configurable PID/control gains**
* **Robot-specific YAML configuration**
* **Action-sequence based task execution**
* **Insertion task simulation**
* **Force and admittance experiments**
* **Input-device based teleoperation**
* **3D SpaceMouse support**
* **PS Move controller support**
* **MuJoCo visualization**
* **Robot and environment mesh support**
* **Example manipulation tasks**
* **Testing infrastructure for controllers and robot behavior**

The original IRL Control framework specifically provides operational-space control, admittance control, dual-arm UR5 manipulation, teleoperation, configurable YAML-based robot parameters, and insertion-task support.

---

## Repository Structure

```text
samriddhi_irl_control/
│
├── .github/
│   └── workflows/
│
├── img/
│   └──                  # Images and visual assets
│
├── irl_control/
│   │
│   ├── action_sequence_configs/
│   │   ├── insertion_task.yaml
│   │   └── iros2022_task.yaml
│   │
│   ├── examples/
│   │   ├── admit_test.py
│   │   ├── force_test.py
│   │   ├── gain_test.py
│   │   ├── insertion_task.py
│   │   ├── ps_move_example.py
│   │   └── space_mouse_example.py
│   │
│   ├── input_devices/
│   │
│   ├── meshes/
│   │
│   ├── robot_configs/
│   │   ├── default_xyz.yaml
│   │   ├── default_xyz_abg.yaml
│   │   └── iros2022.yaml
│   │
│   ├── scenes/
│   │
│   ├── tests/
│   │
│   ├── device.py
│   ├── mujoco_app.py
│   ├── osc.py
│   ├── robot.py
│   ├── utils.py
│   └── version.py
│
├── .gitignore
├── LICENSE.txt
├── requirements.in
├── setup.py
└── README.md
```

The current repository contains dedicated directories for action sequences, examples, input devices, meshes, robot configurations, MuJoCo scenes, and tests.

---

## Main Components

### 1. MuJoCo Simulation

MuJoCo is used as the physics simulation environment for the robot and its interaction with objects and the environment.

The simulation framework allows:

* Robot dynamics simulation
* Contact interaction
* External force application
* End-effector motion
* Manipulation task execution
* Visualization and debugging

MuJoCo serves as the physics backbone for simulating interactions between the robot, environment, and manipulated objects.

---

### 2. Operational Space Control

The repository contains an Operational Space Controller for controlling robot end-effectors in Cartesian space.

Operational-space control allows the desired motion of an end-effector to be specified in terms of:

* Position
* Orientation
* Linear velocity
* Angular velocity

The controller uses robot kinematics and Jacobians to transform Cartesian-space commands into robot joint commands.

The implementation is contained primarily in:

```text
irl_control/osc.py
```

---

### 3. Admittance Control

Admittance control is used to allow the robot to respond to external forces.

The basic behavior can be represented using a mass-spring-damper relationship:

[
M\ddot{x} + D\dot{x} + K(x-x_d) = F_{ext}
]

where:

* (M) = virtual mass
* (D) = virtual damping
* (K) = virtual stiffness
* (x) = current end-effector position
* (x_d) = desired position
* (F_{ext}) = external force

This allows the robot end-effector to behave compliantly when interacting with the environment.

The repository includes an admittance-control example:

```text
irl_control/examples/admit_test.py
```

The original IRL framework demonstrates this behavior by applying external forces to a dual-arm UR5 and observing the spring-damper response.

---

## 4. Bimanual Manipulation

The framework is designed around coordinated control of two robot arms.

A bimanual system can be represented as:

```text
                 Manipulated Object
                  /              \
                 /                \
        Left Robot Arm       Right Robot Arm
              |                    |
          Controller           Controller
              \                    /
               \                  /
                ---- MuJoCo -----
```

Both arms can be controlled independently or coordinated through a common manipulation task.

This structure makes the framework suitable for research involving:

* Cooperative manipulation
* Object transportation
* Contact-rich manipulation
* Assembly
* Insertion
* Force interaction
* Dual-arm coordination

---

## 5. Insertion Task

One of the main examples included in the repository is the insertion task.

```text
irl_control/examples/insertion_task.py
```

The corresponding task configuration is:

```text
irl_control/action_sequence_configs/insertion_task.yaml
```

The insertion task involves precise alignment and insertion of components under tight positional and orientation constraints.

The action sequence defines task stages such as:

1. Move toward the object
2. Approach the grasp location
3. Grasp the object
4. Move toward the insertion location
5. Align the object
6. Perform insertion
7. Release the object

The original IRL implementation uses NIST male/female adapters for low-tolerance insertion experiments.

---

## 6. Force Interaction

The repository contains a force-interaction example:

```text
irl_control/examples/force_test.py
```

This example demonstrates how the robot responds when interacting with an external surface.

Instead of continuously pushing against an obstacle with large forces, compliant control allows the robot to respond dynamically to contact.

This is useful for studying:

* Contact forces
* Compliance
* Stability
* Collision response
* Interaction control

---

## 7. Controller Gain Testing

Controller parameters can significantly influence robot stability and tracking performance.

The repository provides:

```text
irl_control/examples/gain_test.py
```

for evaluating controller gains.

The robot configuration files contain parameters such as:

* PID gains
* Velocity limits
* Kinematic parameters
* Robot-specific control settings

Available configurations include:

```text
irl_control/robot_configs/
├── default_xyz.yaml
├── default_xyz_abg.yaml
└── iros2022.yaml
```

---

## 8. Input Devices

The framework supports external input devices for teleoperation and interactive manipulation.

Examples include:

* PS Move controllers
* 3Dconnexion SpaceMouse

Example programs are available at:

```text
irl_control/examples/ps_move_example.py
irl_control/examples/space_mouse_example.py
```

These interfaces can be used to control robot end-effectors interactively and are useful for collecting demonstrations and testing manipulation behavior.

The original framework provides one-to-one correspondence between input devices and robot arms, enabling extension to more complex bimanual teleoperation tasks.

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/Samriddhi-dubey06/samriddhi_irl_control.git
cd samriddhi_irl_control
```

### 2. Create a Python virtual environment

It is recommended to use a dedicated virtual environment.

```bash
python3 -m venv irl_control
```

Activate it using:

### Linux / macOS

```bash
source irl_control/bin/activate
```

### Windows

```bash
irl_control\Scripts\activate
```

---

### 3. Install the package

Install the repository in editable mode:

```bash
pip install -e .
```

---

### 4. Install dependencies

The repository currently specifies the following Python dependencies:

```text
numpy
PyYAML
transforms3d
```

These are listed in `requirements.in`.

Install them with:

```bash
pip install -r requirements.in
```

---

## Running the Examples

After installation, the examples can be executed from the repository root.

### Admittance Control Test

```bash
python -m irl_control.examples.admit_test
```

### Force Interaction Test

```bash
python -m irl_control.examples.force_test
```

### Gain Test

```bash
python -m irl_control.examples.gain_test
```

### Insertion Task

```bash
python -m irl_control.examples.insertion_task
```

### PS Move Teleoperation

```bash
python -m irl_control.examples.ps_move_example
```

### SpaceMouse Teleoperation

```bash
python -m irl_control.examples.space_mouse_example
```

> Depending on the installed MuJoCo version, Python environment, input-device drivers, and local system configuration, additional system dependencies may be required for visualization or hardware-based input devices.

---

## Configuration

Robot and controller parameters are stored in YAML configuration files.

For example:

```text
irl_control/robot_configs/
```

contains:

```text
default_xyz.yaml
default_xyz_abg.yaml
iros2022.yaml
```

Task-level action sequences are stored separately:

```text
irl_control/action_sequence_configs/
```

Currently available task configurations include:

```text
insertion_task.yaml
iros2022_task.yaml
```

This separation makes it possible to modify robot/controller parameters without changing the main control code.

---

## Action Sequences

Action sequences provide a structured way to describe multi-stage manipulation tasks.

For example:

```text
Approach
   ↓
Grasp
   ↓
Move
   ↓
Align
   ↓
Insert
   ↓
Release
```

The action sequence configuration specifies task-dependent information such as:

* Robot arm selection
* Object information
* Target location
* Position/orientation constraints
* Force requirements
* Velocity constraints
* Task stages

The repository currently includes both insertion and IROS 2022 task configurations.

---

## Project Architecture

```text
                    ┌─────────────────────────┐
                    │      Task / Example     │
                    │ insertion_task.py       │
                    │ force_test.py           │
                    │ admit_test.py           │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    Action Sequence      │
                    │       YAML Config       │
                    └────────────┬────────────┘
                                 │
                                 ▼
              ┌────────────────────────────────────┐
              │          Robot Controller           │
              │                                    │
              │  Operational Space Control (OSC)  │
              │  Admittance Control                │
              │  PID / Gain Configuration          │
              └────────────────┬───────────────────┘
                               │
                               ▼
                    ┌─────────────────────────┐
                    │      Robot Model        │
                    │      robot.py           │
                    │      device.py           │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │        MuJoCo            │
                    │   Physics + Contacts     │
                    │   Robot + Environment    │
                    └─────────────────────────┘
```

---

## Typical Research Workflow

A typical workflow using this repository is:

```text
1. Define robot configuration
          ↓
2. Define MuJoCo scene
          ↓
3. Configure controller gains
          ↓
4. Define manipulation task
          ↓
5. Create action sequence
          ↓
6. Run simulation
          ↓
7. Evaluate tracking/contact behavior
          ↓
8. Tune controller parameters
          ↓
9. Repeat experiments
```

This structure makes the framework suitable for iterative robotics research and controller development.

---

## Applications

This framework can be used for research and experimentation in:

* Bimanual manipulation
* Robotic assembly
* Insertion tasks
* Contact-rich manipulation
* Compliant manipulation
* Admittance control
* Operational-space control
* Robot teleoperation
* Demonstration collection
* Controller evaluation
* MuJoCo-based robotics research
* Simulated manipulation experiments

---

## Development

The main package is organized around several core modules:

```text
robot.py
```

Robot abstraction and robot-level functionality.

```text
osc.py
```

Operational-space control functionality.

```text
device.py
```

Device and control-interface functionality.

```text
mujoco_app.py
```

MuJoCo application and simulation functionality.

```text
utils.py
```

Utility functions used throughout the framework.

```text
tests/
```

Testing infrastructure.

The repository is packaged using `setup.py` and identifies itself as `irl_control`. The package description is **"Control Suite for Bi-Manual Manipulation tasks in Mujoco."**

---

## Research Direction

The framework can serve as a foundation for developing more advanced bimanual manipulation methods, including:

* Learning-based manipulation
* Imitation learning
* Reinforcement learning
* Force-aware manipulation
* Contact-aware trajectory generation
* Dual-arm coordination
* Object-level control
* Sim-to-real manipulation
* Robust insertion
* Compliance-aware control

Because task descriptions, robot configurations, control algorithms, and MuJoCo scenes are separated, new experiments can be developed without restructuring the entire framework.

---

## Acknowledgements

This repository is based on the **IRL Control** framework developed by the Interactive Robotics Lab at **Arizona State University**.

Original project:

https://github.com/ir-lab/irl_control

The original project provides a MuJoCo control suite for bimanual manipulation, including operational-space control, admittance control, teleoperation, configurable robot parameters, and insertion-task experiments.

---

## License

This project is distributed under the **MIT License**. See [`LICENSE.txt`](LICENSE.txt) for details.

---

## Author

**Samriddhi Dubey**

Repository maintained for robotics research, MuJoCo simulation, bimanual manipulation, and control development.

---

## Citation



```bibtex
@software{irl_control,
  title  = {IRL Control: MuJoCo Control Suite for Bimanual Manipulation},
  author = {Interactive Robotics Lab},
  url    = {https://github.com/ir-lab/irl_control}
}
```

---

## Repository

**GitHub:**
https://github.com/Samriddhi-dubey06/samriddhi_irl_control

**Original IRL Control:**
https://github.com/ir-lab/irl_control

