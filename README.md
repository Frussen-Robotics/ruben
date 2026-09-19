# Ruben

Ruben is the first embodied AI agent developed by **Frussen Robotics**.

The project aims to build a persistent AI agent that can converse, retain useful context, perceive its surroundings, and interact with the physical world through compatible robotic bodies.

Ruben is the **agent**, not the robot hardware itself.

Its first physical body is a **Hiwonder MentorPi M1 Advanced**, but the agent is intended to remain independent from any specific robot platform.

## Project status

Ruben is currently in the early architecture and integration phase.

The first physical platform, a Hiwonder MentorPi M1 Advanced, has been received and its main vendor-provided functionality has been verified, including:

* Raspberry Pi 5
* ROS 2 environment
* RGB/depth camera
* LiDAR
* mecanum drive
* local network control

The architecture is still evolving, and implementation is intentionally proceeding in small vertical slices.

## Architecture

The current conceptual model is:

```text
                    RUBEN
              Agent / OpenClaw
                     │
                     │ body capabilities
                     ▼
             compatible body runtime
                     │
                     ▼
              robot software stack
                     │
                     ▼
                  hardware
```

OpenClaw is planned as the initial agent runtime for Ruben, providing agent-side configuration, tools, memory integration, and access to AI models.

Physical control remains outside the agent runtime.

Ruben should express high-level intentions to a body rather than directly controlling hardware or publishing arbitrary ROS commands.

For example:

```text
Ruben
  │
  │ rotate_by(360°)
  ▼
MentorPi runtime
  │
  ├── execute local control loop
  ├── observe odometry / IMU
  ├── control ROS motion
  └── stop when complete
```

A useful design principle is:

> **The agent expresses intentions; the body handles control.**

## Bodies

A physical body exposes a controlled set of high-level capabilities to Ruben.

Possible capabilities include:

```text
get_status()
observe_scene()
stop()
rotate_by(...)
navigate_to(...)
```

Different bodies do not need to expose exactly the same capabilities.

The body implementation is responsible for translating these intentions into platform-specific behavior.

For the MentorPi, this may involve ROS 2, Hiwonder drivers, odometry, IMU data, mecanum motion, LiDAR, camera streams, and eventually Nav2.

Those implementation details should remain hidden from Ruben.

## MentorPi

MentorPi-specific software is maintained separately from the Ruben agent in:

```text
Frussen-Robotics/mentorpi
```

The `mentorpi` repository is intended to contain the software required to expose the Hiwonder MentorPi as a controlled robotic body.

Its responsibilities may progressively include:

* ROS 2 integration
* robot state
* high-level body capability API
* motion supervision
* action execution
* sensor access
* local safety behavior
* MentorPi-specific deployment and configuration

The MentorPi runtime should remain usable and testable independently from Ruben and OpenClaw.

## Future bodies

Ruben should remain the same agent when attached to a different compatible body.

For example:

```text
Frussen-Robotics/
├── ruben
├── mentorpi
└── microduck
```

A future `microduck` implementation could expose a different capability set without requiring a separate copy of the Ruben agent.

Body-specific behavior may eventually be added to Ruben where there is a concrete need, but separate agents such as `ruben-mentorpi` or `ruben-microduck` should not be created merely because the physical platform changes.

## Scope of this repository

`Frussen-Robotics/ruben` contains the software and configuration that define **Ruben as an agent**.

Its future scope may include:

* OpenClaw workspace and Ruben-specific configuration
* agent instructions and identity
* tools and skills
* memory configuration
* voice and conversation integration
* body capability clients
* documentation describing Ruben's behavior and architecture

Runtime-generated OpenClaw data, credentials, logs, caches, sessions, and other machine-specific state should not be committed.

The exact OpenClaw repository layout will be introduced only after the runtime has been installed and its actual structure has been inspected.

## Engineering principles

* Keep Ruben independent from any specific physical body.
* Do not give AI models arbitrary direct access to ROS or motors.
* Expose physical actions through narrow, controlled capabilities.
* Keep realtime control loops local to the body.
* Keep stopping and safety mechanisms independent from cloud AI models.
* Allow a body to complete an accepted physical action without continuous commands from the agent.
* Preserve vendor-provided robot systems where practical.
* Introduce abstractions only when concrete implementations justify them.
* Avoid unnecessary microservices and distributed infrastructure.
* Build one small vertical slice at a time.
* Understand and validate behavior even when AI writes much of the implementation.

The immediate next step is to begin the separate MentorPi body runtime and expose its first minimal capability.
