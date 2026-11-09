---
layout: project
title: MarshBot - Amphibious Wetland Robot
description: Low-cost amphibious robot for freshwater wetland research, presented at NERC 2026
technologies: [Fusion 360, 3D printing, Raspberry Pi, RTK GNSS, IMUs, Waterproofing, Research]
image: /assets/images/marshbotreal.png
---

## Overview

As part of my research in the Collective Embodied Intelligence Lab at Cornell, Hilario Esparza, Professor Kirstin Petersen, and I have been developing a low-cost amphibious robot for freshwater wetland research.

Wetlands are large carbon sinks and produce 20-30% of total annual global methane emissions, so understanding them better improves our understanding of climate. However, wetlands are notoriously difficult to navigate, and existing robotic platforms that can explore them tend to be expensive and inaccessible to researchers. Our goal is to create an inexpensive, customizable robot that would allow ecologists to map and monitor wetlands at a greater scale than conventional field measurements.

We presented our work at the Northeast Robotics Colloquium (NERC 2026).

---

## Challenges

Designing for wetlands came with a few main challenges:

- The robot has to operate in remote locations where there may be no cellular service.
- It needs stable motion through rugged terrain, dense growth, and water.
- The electronics need a waterproof enclosure, which is difficult to make while keeping costs low.
- To keep a low barrier-of-entry, the robot can only be built with general-purpose manufacturing equipment and a 3D printer.

---

## Approach

Our design is based on the Velox platform developed by Pliant Robotics. The robot travels in an approximate sinusoidal pattern following a travelling wave, with the legs touching the ground at a minimum of two points of contact per side to propel the robot forward.

Instead of using many individual servo motors, we used a long gear shaft driven by a single DC motor on each side of the robot, which simplifies the control logic. A waterproof box in the center of the chassis holds all the electronics, with rotary seals between the motors and the gear shaft to prevent leaks.

Other key design choices:

- All electronics are off-the-shelf to reduce cost.
- The PLA chassis makes it easy to iterate on the design.
- Separate rechargeable LiPo batteries power the motors and the Raspberry Pi 5.
- A high length-to-width ratio improves efficiency and speed when moving through water.

---

## Electronics and Sensors

The waterproof box contains the Raspberry Pi, two DC motors and motor controllers, an RTK GNSS sensor, and two IMUs.

- **IMUs:** Two IMUs mounted on orthogonal axes are used to estimate the pitch, roll, and yaw of the robot.
- **GNSS:** The ZED-F9P-05B is a high-precision GNSS module with RTK, which lets the operator send correctional data to the robot for centimeter-level accuracy.
- **Cameras:** A forward-facing camera streams video to the operator's computer, and a second camera faces beneath the robot. This lets the operator safely drive the robot without needing to see it directly.

---

## Skin

We are also developing a waterproof, easy-to-assemble skin that wraps around the entire robot. The skin gives the robot "fins" to move through water, increases traction on land, and adds a first layer of protection for the electronics. We are testing both TPU, which is printable and flexible, and silicone, which is harder to reproduce but would likely be more water resistant.

---

## Current Prototype

Here is our current real-life prototype:

![Photo of MarshBot prototype]({{ "/assets/images/marshbotreal.png" | relative_url }}){: .inline-image }

---

## Future Work

Our next steps include:

- Fully waterproofing the electronics box with epoxy
- Developing a method for making the fins and testing locomotion in water and on land
- Adding a customizable space for ecological sensors such as moisture, pH, and oxygen saturation sensors
- Redesigning the chassis to be easier to build and use less material
- Developing basic waypoint navigation and state-space control

---

## NERC 2026

Here is me (left) and my collaborator Lalo Esparza presenting our robot and poster at NERC 2026:

![Ryan and Lalo Esparza presenting at NERC 2026]({{ "/assets/images/marshbotnerc.png" | relative_url }}){: .inline-image }

[Download photo]({{ "/assets/marshbotnerc.pdf" | relative_url }}) in PDF format.

---

## Documentation

Here is our NERC 2026 poster:

![MarshBot NERC 2026 poster]({{ "/assets/images/marshbotposter.png" | relative_url }}){: .inline-image }

[Download poster]({{ "/assets/marshbotposter.pdf" | relative_url }}) in PDF format.
