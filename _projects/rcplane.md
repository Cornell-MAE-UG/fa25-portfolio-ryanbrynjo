---
layout: project
title: RC Plane CAD
description: An RC Plane with XFoil wing shape
technologies: [Fusion 360, XFoil]
image: /assets/images/plane.png
---

## Overview

In the summer of 2024, I was lacking any CAD experience. Because of my love of aviation and space, I thought it would be fun and challenging to learn how to use CAD while designing a sleek and build-able RC plane.

To begin, I watched a couple of videos on planes and how to begin designing them. To give myself a crutch, I decided to follow a blue-print of a random stunt-plane. I followed this blueprint to give a rough outline of the plane shape from top and bottom views and side views. I decided my plane would be a maximum of 50 cm long and 35 cm wide.

---

## Initial Shape

Here is my plane's initial model after blueprints:

![Photo of initial plane shape]({{ "/assets/images/planeinitial.png" | relative_url }}){: .inline-image }

---

## Airfoil Selection Using XFoil

Initially for my airfoil, I was going to use a random standard NACA airfoil. However, I quickly learned about XFoil, a program that can calculate lift and drag coefficients given certain environmental conditions. I used XFoil to optimize which NACA airfoil I selected, and ended up using NACA 4415 because of its high lift coefficient and optimization for low reynolds numbers (laminar flow) at mach # of around 0.1 which would be apparent for a 25-30 mph RC Plane at around 400 feet.

Here is the XFoil analysis I used for a 10 degree tilt airfoil. We note the coefficient of lift is above 1, so it will be sufficient for my plane's operating conditions.

![XFoil Airfoil]({{ "/assets/images/xfoil.png" | relative_url }}){: .inline-image }

![Coefficients of lift]({{ "/assets/images/xfoi2.png" | relative_url }}){: .inline-image }

After I selected this airfoil, I put three cross sections in different regions of the wing aligned with the top view of the wing profile (scaled so top and bottom of wing section was at wing envelope). Then, I used a path-surface extrusion to model the entire wing. I followed a similar process for my rear elevators. I also used a 10 degree tilt for my wings, and a -2 tilt for my rear elevators.

---

## CAD Modelling Process

Throughout designing the airplane, I mainly utilized surface modelling with paths as guidelines. For a beginner CADDER like myself, this was a rather tough decision; however, given the intricate aerodynamic surfaces of airplanes, I justified this decision and learned along the way.

Some struggles I came across when modelling this airplane was the fact there were multiple regions where the surfaces did not exactly touch. This was my biggest obstacle in turning the surface model into a body, which I could then put into CFD models such as Autodesk CFD to analyze its performance in uniform flow conditions. Unfortunately, although I tried using lofts to connect these surfaces, I was not successful in connecting all regions as there were various error points.

---

## Hand Calculations & Foam Optimization

Because of this obstacle, instead of doing CFD simulations as intended, I decided to switch to hand-calculations given  dimensions of my aircraft and styrofoam's mechanical properties to roughly optimize the shape where I could reduce the weight, eventually finding that a tapered fuselage wall thickness of approximately 4-5 mm provided the best balance between strength and weight for a foam-constructed RC aircraft (reduced weight by 11% as I initially considered a wall thickness of 20 mm).

In this process, I modelled the fuselage as a cylinder and used basic structural beam calculations to understand how loads flowed through the body, helping me determine where extra foam wasn’t actually contributing to maximum stress points (analyzed bending stress and normal stress)

---

## Final Blueprints

Here is my final blueprints from my plane and wings that could be used as base for making aircraft in real life (wing adjusted to real life scale on paper).

**Final Plane Design**

Side View:  
![Photo of final wing blueprint]({{ "/assets/images/plane.png" | relative_url }}){: .inline-image}

Top View:  
![Photo of final wing blueprint]({{ "/assets/images/plane2.png" | relative_url }}){: .inline-image}


**Final Plane Blueprint:**

![Photo of final wing blueprint]({{ "/assets/images/rcplane2.png" | relative_url }}){: .inline-image }

**Final Wing Blueprint:**

![Photo of wing blueprint]({{ "/assets/images/wingprints.png" | relative_url }}){: .inline-image }

