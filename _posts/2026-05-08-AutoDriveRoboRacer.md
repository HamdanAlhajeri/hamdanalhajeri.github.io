---
title: Autonomous Racing & Robotics Platform
description: An ongoing robotics project combining an ICRA 2026 autonomous racing submission and a modular Senior Design robotics framework — both still actively in development.
author: hamdanalhajeri
date: 2026-05-08 12:00:00 +0000
categories: [Projects, Robotics]
tags: [python, cpp, ros2, lidar, jetson, docker, autonomous]
pin: false
---

# Autonomous Racing & Robotics Platform

> **Work in Progress** — both components of this project are actively being developed and tested.

This page covers two closely related robotics projects: an autonomous racing controller built for the AutoDRIVE RoboRacer, and a modular robotics framework developed as a Senior Design Project (**SDP**) for broader autonomous vehicle research. They share hardware platforms, tooling, and long-term goals.

---

## AutoDRIVE RoboRacer

An autonomous racing controller that uses a LiDAR-based pure pursuit algorithm to navigate a race track without human intervention, built on ROS 2 and containerized with Docker.

### How It Works

The controller subscribes to a 270° LiDAR LaserScan, parses the point cloud into Cartesian coordinates, separates wall points, computes lateral offset, and steers the vehicle toward a lookahead point. Throttle is dynamically scaled inversely with steering magnitude so the car slows down automatically in corners.

### Key Features

- **Pure Pursuit Controller** — wall-following algorithm with tunable lookahead distance
- **Dynamic Speed Control** — throttle decays with steering angle for safer cornering
- **ROS 2 Integration** — subscribes to `/scan`, publishes throttle and steering commands
- **Containerized** — Docker and Docker Compose for both production and live-edit development

### Tunable Parameters

| Parameter | Default |
|-----------|---------|
| Lookahead distance | 0.8 m |
| Max throttle | 0.6 |
| Steering gain | 1.2 |
| Throttle decay | 2.5 |

---

## Senior Design Project (SDP) — Autonomous Robotics Framework

A modular robotics framework for autonomous vehicle research. Components are designed to be independent and reusable, currently focused on low-latency RC vehicle teleoperation using a Jetson board, with planned expansion into vision, sensor fusion, and path planning.

### How It Works

Rather than routing serial commands through an Arduino, the system drives GPIO pins directly from the Jetson at 50 Hz — reducing latency and simplifying the hardware stack. A USB gamepad provides input and the board generates PWM signals suitable for servo and ESC control.

### Key Features

- **Direct GPIO PWM** — 50 Hz PWM from Jetson pins without an Arduino intermediary
- **USB Gamepad Teleoperation** — configurable axis and button mapping
- **Emergency Stop** — hardware-level safety cutoff
- **Modular Architecture** — each capability lives in its own self-contained module

### Planned Modules

- Vision processing
- Sensor fusion
- Path planning
- Localization

---

## Shared Tech Stack

| Area | Tools |
|------|-------|
| Languages | Python, C++ |
| Frameworks | ROS 2 |
| Hardware | NVIDIA Jetson, LiDAR |
| Containerization | Docker / Docker Compose |

## Repositories

- [AutoDRIVE RoboRacer](https://github.com/HamdanAlhajeri/autodrive-roboracer)
- [SDP](https://github.com/HamdanAlhajeri/SDP)
