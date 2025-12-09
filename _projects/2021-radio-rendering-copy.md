---
layout: project
title: ANSYS Finite Element Analysis for Torque Wrench
description: Building a rocket nozzle solver for diverging section
technologies: [Python, ANSYS, Finite Element Analysis, Optimization]
image: /assets/images/torquewrench1.png
---

# Overview
For my final project in Mechanics of Materials at Cornell University, we were tasked with designing and optimizing a torque wrench given a design torque to pass certain constraints. These constraints were relating to fracture mechanics, normal stress, and fatigue, with an extra constraint related to strain gauge bridge output.

The project outline started with hand calculations using standard beam theory in Python, and later moved to ANSYS to confirm beam theory calculations and get more exact estimate; using the combination of values from ANSYS and hand calculations, I then optimized the design to minimize stress concentrations and retested values in ANSYS while meeting design conditions.


# Material Properties    

**MATERIAL USED:**    

M42 TOOL STEEL:    

a cobalt-alloyed high-speed steel known for very high hardness, strength, and wear resistance, commonly used in cutting tools for high-stress applications.    

Relevant Properties:    
   
Young’s Modulus E = 32.1*10**6  psi    

Yield Strength = 343 ksi    

Tensile Strength = 374  ksi    

v = 0.285  poisson ratio    

Stress Amplitude  = 115 ksi (fatigue strength at 1e6 cycles)    

Fracture Toughness = 14.1  ksi*sqrt(inch)    

---

# Pictures of Initial Design:

**Beam Theory**   

![Photo of outline]({{ "/assets/images/rough.png" | relative_url }}){: .inline-image }   

This is the initial design I considered in my calculations. We see this is a simple design only crudely modelling a torque wrench. It is noted length L is not the entire length of the torque wrench, but the distance to where the point force is applied from the center of the drive.

Code for these calculations can be found at the bottom of the page.

Here is the CAD Model I originally created:     

![Photo of outline]({{ "/assets/images/ye.png" | relative_url }}){: .inline-image }     

---

# Photos of Improved CAD Model  

-I changed my wrench to have smoother contours to mitigate stress concentrations. I also made my wrench have a handle (by making it longer) that has a wider cross section to ensure it does not interfere with maximum stress locations; I assumed the point force was applied at the end of the handle in the same location as my hand calculations.    

**Pictures of Updated Model**      

Side View:    

![Photo of torque wrench side view]({{ "/assets/images/sideview.png" | relative_url }}){: .inline-image }     

Top View:   

![Photo of torque wrench top view]({{ "/assets/images/topview.png" | relative_url }}){: .inline-image }    

**Dimensions**   

![Photo of torque wrench dimensions]({{ "/assets/images/torquedimension.png" | relative_url }}){: .inline-image }     



An issue I ran into was a way to smoothen the transition from the rectangular drive surface to the now circular wrench profile, as this is where the maximum stress points were. I tried many loft, fillet, and custom spline curves only to end up making a semi-smooth transition. While this was an improvement over the previous design, it still introduces a region of high stress due to the sharp edges.

 
---   

# ANSYS Finite Element Analysis   

**Loading Conditions**    

Here are the loading conditions I used for ANSYS:    

![Photo of loading conditions]({{ "/assets/images/loading.png" | relative_url }}){: .inline-image } 

I assume a clamped boundary condition at the drive and a point force of 37.5 lbf into the page for a length to the drive of 16 inches, generating the required torque of 600lbf*in.


**Contour Plots**   

Normal Strain Contours in Strain Gauge Direction:   

![Photo of strain contours]({{ "/assets/images/strain.png" | relative_url }}){: .inline-image }   

Maximum Principle Stress Contours:   

![Photo of principle stress contours]({{ "/assets/images/maxprinc.png" | relative_url }}){: .inline-image }   

**Summary of ANSYS Results:**   

My strain gauges are set an inch in the Y direction from the middle of the drive. To analyze these points, I set a coordinate system where the middle of the strain gauges were (.45" x .23" rectangular boxes) and used a probe for normal strain in the Y direction. I followed a similar process to measure for the deflection of the point load.  


LOAD POINT DEFLECTION: .15845 inches   

MAX NORMAL STRESS: 18366 psi    
 
STRAINS AT GAUGE LOCATION: 504.5 microstrain    

To analyze my torque wrench sensitivity, I used my microstrain measured at my strain gauges and worked through the conversion:   

From 504.5 microstrain:  

For a transduction factor G_f of 2:   

Bridge ratio = G_f * Strain, so 2*(504.5*10^-6 strain) = 0.001009. Multiply by 1000 to get mV/V: = 1.009 mV/V. This assumes a full bridge setup, where I have 2 strain gauges on the compressive side, and 2 strain gauges on the tensive side of the bending. This meets the design requirement for bridge output.   

**The exact strain gauge used:**   

A foil resistance strain gauge suitable for bending in a full Wheatstone bridge configuration   

Manufacturer / Series: Vishay Micro-Measurements – General Purpose Foil Gauge    
 Gauge Type: Uniaxial constantan foil strain gauge    
 Model Equivalent: EA-06-250BF-120 (industry standard)     

Gauge Factor G_f: 2.0


Temperature Compensation: Steel / steel alloys


Physical Dimensions
Active Grid Length: 0.25 in    


Active Grid Width: 0.12 in     


Total Backing Length: 0.45 in     


Total Backing Width: 0.23 in     


Backing Thickness: 0.001 in    

We note this strain gauge has enough room to measure our strains.    


---

# Beam Theory Calculations Code
Here is the single-file code I used to iterate my design to meet requirements:

```python
import numpy


"""
Design Parameters (ryan brynjolfson rrb244)
"""


## Torque Wrench Parameters
# all in inches


L = 16
h = 0.59
b = 0.6
c = 1
crackdepth = 0.04
I_w = (b * h**3) / 12    
J_w = (b*h**3 + h*b**3)/12  


# drive dimensions
drive = 3/8
driveheight1 = 0.4
driveheight2 = 0.5
J_d = .1406*(3/8)**4  # Polar Moment of Inertia of drive
I_d = ((3/8)**4)/12   # Bending moment of inertia of drive


# Given conditions/Constraints
T = 600  # in-lbf
N = 10**6  # cycles
voltage = 1.0  # mV/V
X_o = 4   # safety factor for yield failure (ductile material)
X_k = 2   # safety factor for crack growth (0.04 in)
X_s = 1.5 # safety factor for fatigue stress


## M42 Properties


E = 32.1*10**6      # psi
yield_strength = 343  # ksi
tensile_strength = 374  # ksi
v = 0.285           # poisson ratio
stress_amp = 115    # ksi (fatigue strength at 1e6 cycles)
fracture_toughness = 14.1  # ksi*sqrt(inch)
toughness = 0.504   # ft*lbf/in^2


#Stresses


# drive torsion
tau = (T*(3/8)) / J_d  # shear stress, psi
tau_ksi = tau / 1000


#end load
P = T / L # lbf


# distance from neutral axis to outer fiber
c_f = h / 2


# Max bending stress at drive end
sigma_max = T * c_f / I_w       # psi
sigma_max_ksi = sigma_max / 1000


# Bending moment and stress at gauge location
x_gauge = L - c # distance from drive to gauge
M_gauge = P * x_gauge # in-lbf
sigma_g = M_gauge * c_f / I_w # psi at gauge
sigma_g_ksi = sigma_g / 1000


# Safety Factors


# Strength SF
SF_yield = yield_strength / abs(sigma_max_ksi)


# Fatigue SF (fully rev)
SF_fatigue = stress_amp / abs(sigma_max_ksi)


# Fracture mechanics
Y = 1.12
K_I = Y * abs(sigma_max_ksi) * numpy.sqrt(numpy.pi * crackdepth)  # ksi*sqrt(in)
SF_fracture = fracture_toughness / K_I


#Strain & Bridge Output
gauge_factor = 2.0
epsilon = (sigma_g) / E  


bridge_ratio = gauge_factor * epsilon  # full bridge
output_mV_per_V = bridge_ratio * 1000.0


#deflection of end


delta = P * L**3 / (3 * E * I_w) #in








# results


print("Shear stress in drive (not normal stress, extra info): {:.3f} ksi".format(tau_ksi))


print("Max normal stress at drive (bending): {:.3f} ksi".format(sigma_max_ksi))
print("Bending stress at gauge: {:.3f} ksi".format(sigma_g_ksi))
print("Gauge strain = {:.1f} microstrain".format(epsilon * 1e6))
print()
print("Safety Factors")
print("Static yield SF (required >= {}): {:.2f}".format(X_o, SF_yield))
print("Fatigue SF (required >= {}): {:.2f}".format(X_s, SF_fatigue))
print("Fracture SF (required >= {}): {:.2f}".format(X_k, SF_fracture))
print()


print("Gauge strain = {:.1f} microstrain".format(epsilon * 1e6))
print("Predicted bridge output = {:.2f} mV/V (required >= {:.2f} mV/V)"
      .format(output_mV_per_V, voltage))
print(F"Handle point deflection = {round(delta,3)} in")

```


