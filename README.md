# Ruben

Ruben is the first project of **Frussen Robotics**: an experimental domestic social robot based on the Hiwonder MentorPi M1.

The project aims to build an **embodied AI** platform that can progressively perceive its surroundings, hold conversations, retain useful context, and perform controlled physical actions. Navigation is one part of that broader goal. These capabilities are planned; they are not implemented in this repository yet.

## Project status

Ruben is in the **pre-hardware / bootstrap** phase. This repository currently contains introductory documentation only. It does not yet provide robot software, capability APIs, deployment services, or an installation procedure.

The architecture is still evolving. Hardware integration and compatibility with the vendor-provided system remain to be verified on the target robot.

## Target hardware

The initial target platform is the **Hiwonder MentorPi M1**, with:

- Raspberry Pi 5
- ROS 2 and the vendor-provided robot stack
- Camera
- LiDAR
- Mecanum drive base

The initial approach is to preserve the original Hiwonder system and build on top of it, rather than immediately reinstalling or replacing the vendor stack. Exact software versions, interfaces, and setup requirements will be documented as they are verified.

## Scope of this repository

`Frussen-Robotics/ruben` is intended to be the main repository for the robot-side software needed to turn a compatible MentorPi into Ruben. Its planned scope includes:

- ROS 2 integration and access to robot state
- Local, controlled capability APIs
- Local services and configuration
- Deployment and installation documentation

The long-term intent is to let other MentorPi owners install and run Ruben on their own robots. There is no supported installation path yet. Code, dependencies, tooling, and directories will be introduced only when there is a concrete need for them.

## Planned architecture

The architecture separates four responsibilities. These are conceptual boundaries, not a set of implemented services or a requirement to create a service for each layer.

| Layer | Intended responsibility |
| --- | --- |
| Robot / body | ROS 2 integration, drivers, perception, navigation, robot state, safety, and motion control. |
| Capability / bridge | Narrow local APIs that expose controlled robot capabilities without granting AI components direct access to ROS or motors. |
| Conversation | Future integration with the OpenAI Realtime API for low-latency voice conversation. |
| Agent / OpenClaw | OpenClaw workspace and agent-side configuration, maintained in a separate repository. |

The robot / body and capability / bridge layers form the core scope of this repository. The conversation integration is planned; its implementation details and placement are still to be determined.

## Relationship with OpenClaw

OpenClaw will **not** live in this repository. Its workspace will have a distinct structure and lifecycle, maintained in a separate repository, `Frussen-Robotics/ruben-openclaw`.

That repository is intended to contain the OpenClaw workspace and agent-side configuration. It will interact with the robot through the controlled capabilities exposed by Ruben, without direct access to ROS or motors. The interface between the repositories has not yet been implemented or finalized.

## Initial engineering principles

The following principles guide future implementation; they are not claims about safeguards already implemented here:

- Preserve the vendor-provided Hiwonder / ROS stack initially and build on top of it.
- Do not give the AI model direct access to ROS or motors.
- Allow only one active motion source at a time.
- Keep stopping and safety mechanisms local and independent of the cloud model.
- Avoid unnecessary distributed buses and microservices.
- Keep the initial design minimal and introduce structure and dependencies only when they are needed.
