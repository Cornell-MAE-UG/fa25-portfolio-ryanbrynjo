---
layout: project
title: Cardboard Carnivore - MAE 3780 Robot Competition
description: Autonomous cube-collecting robot designed and programmed for the MAE 3780 class competition
technologies: [Arduino, TCS3200 Color Sensor, H-Bridge Motor Control, Fusion 360, Cardboard Fabrication, Embedded C]
image: /assets/images/cardboardcarnivore.png
---

## Overview

As part of MAE 3780, my group members Andrew Fujimoto, Nate Jennewein and I (Group 43 — **Cardboard Carnivores**) designed, built, and programmed an autonomous robot for a cube-collecting competition. 

Our guiding philosophy was **“Keep it simple, stupid.”** We kept the mechanical design deliberately minimal — a standard chassis with fixed arms forming a cube-capturing perimeter — so we could focus our effort on a robust software strategy that would let us outdrive the competition. Instead of adding complex servo mechanisms to extend beyond the 8″ box limit, we invested time in reliable color-based navigation using the TCS3200 sensor.

The robot performed a  30 second autonomous loop to collect cubes, then stopped and “survived” against other robot for the remainder of the match.

On the Cornell Robot Competition sponsored by ASML, our team took 5th/62 teams with a final record of 5-2-1!



## Initial Prototypes

**Prototype 1:** Minimum viable driving platform — chassis, motors, wheels, Arduino, breadboard, batteries, and two H-bridges. This let us complete the first milestone with hard-coded movements.

**Prototype 2:** Identical to Prototype 1 but with the addition of the TCS3200 color sensor. We initially mounted it using a cardboard buffer and duct tape for rapid testing before securing it properly with M3 standoffs and screws.

**Prototype 3 (Final design):** We attached the arms to the *rear* of the chassis (a deviation from our original Fusion 360 CAD) to extend the capturing perimeter by 3 inches and dramatically increase our scoring ceiling. The arms were built from cardboard, reinforced with M3 standoffs, hot glue, and extra layers. The cardboard proved far more durable than expected, so we never printed the CAD parts.

![Cardboard Carnivore final robot]({{ "/assets/images/cardboardcarnivore" | relative_url }}){: .inline-image }

## CAD and Fabrication

Our original mechanical design was drafted in Fusion 360 (isometric and top views shown in the report). Due to time lost troubleshooting the color sensor, lab-space constraints, and busy schedules, we made the pragmatic decision to use cardboard, hot glue, and lab hardware instead of 3D printing. This saved significant time and money with virtually no performance penalty for the competition environment.

**Electrical design** was equally minimalist: each motor wired to an H-bridge (per Lab 3) and the TCS3200 color sensor wired per Lab 4. All wires from the breadboard were taped together to prevent accidental disconnection during the match.

## Software and Strategy

The robot used the TCS3200 color sensor to drive a short autonomous cube-collecting loop:
- Drives straight by default.
- A color change (blue ↔ yellow) triggers a 90° turn to follow the yellow/blue border.
- Detection of the black outer edge causes the robot to stop and terminate the autonomous phase.

Afterward it simply sits still for the rest of the match.

**Our competition strategy** was intentionally straightforward: start in the middle, drive straight, turn right onto the centerline after a short delay, collect as many cubes as possible, then stop and survive. The goal was to secure more than half the blocks before the opposing robot.

![Robot algorithm flowchart]({{ "/assets/images/flowchart.png" | relative_url }}){: .inline-image }

Full Arduino code is included in Appendix E of the final report.

## Project Reflection

Multiple barriers — limited lab space, busy personal schedules, and a long troubleshooting process with the TCS3200 color sensor — caused our final robot to look quite different from our original proposal. These constraints forced us to accelerate other parts of the design, most notably by using cardboard instead of 3D-printed parts.

The biggest lesson was **knowing when to cut your losses**. We could have spent many more hours chasing a “complete” solution, but an 80 % solution struck the perfect balance between effectiveness and efficiency. Many of the changes we made due to constraints (cardboard arms, simplified software) had almost no negative impact on performance. In fact, the simpler code was far easier to write, debug, and make reliable — and it became a key factor in our success.

Overall, the Cardboard Carnivore was an excellent reminder that more complex systems do not necessarily produce a more robust final result. Prioritizing simplicity when possible is often the smartest engineering decision.

## Documentation

- **[Full MAE 3780 Final Report PDF]({{ "/assets/mae3780-final-report.pdf" | relative_url }})** (includes appendices with Bill of Materials, Circuit Diagram, CAD reference images, algorithm flowchart, and complete Arduino code)
- Rendering / CAD files available upon request
- Group members: Ryan Brynjolfson, Andrew Fujimoto, Nate Jennewein