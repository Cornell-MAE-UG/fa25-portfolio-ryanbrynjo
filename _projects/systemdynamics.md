---
layout: project
title: CU-D2 Self Balancing Robot
description: Using System Dynamics, Mujoco, and Dynamics to make a self balancing robot
technologies: [Python, Dynamic Modelling, State-Space Modelling, MuJoco]
image: /assets/images/system.png

---

# Overview    

For my final project in System Dynamics at Cornell University, we were tasked with applying relevant topics we learned in class to solve a real life problem. What we came up with was CU-D2, a self balancing robot that is a version of the inverted pendulum problem.     

The project outline started with a dynamic model, which we then used to make a state-space representation of the system. Then we defined control laws and references and made a computer simulation.    

I did this project with Cornell University Students Charles Wall-Davis, Alan Muschy, and Bryan Kim.

---

# Dynamic Model

(Ryan Brynjolfson and Charles Wall-Davis)    

Here is a diagram representing how we dynamically modeled our system.    

![Photo of robot diagram]({{ "/assets/images/system.png" | relative_url }}){: .inline-image }   

We defined:     
   
**Inputs:**      
T1 : Motor provided torque on wheel    
T2 : Motor provided torque on linkage    

**States:**    
Position x			 
Theta     
Velocity x'                                     
Angular Velocity      

**Outputs:**       
Position x   
Angular tilt    

After doing the dynamics and linearizing because inverted pendulum is inherently non-linear, we arrived at our governing ODE's.


![Photo of linearized assumption]({{ "/assets/images/linearized.png" | relative_url }}){: .inline-image }   

![Photo of ODES]({{ "/assets/images/odes.png" | relative_url }}){: .inline-image }    

We then solved the coupled ODE's for the acceleration and angular acceleration, and arrived at our state-space model.

We assumed a linear system:    

![Photo of state space]({{ "/assets/images/state.png" | relative_url }}){: .inline-image }    

After this, it is just plugging in numbers. This is our final state-space representation for our system:

![Photo of state space model final]({{ "/assets/images/statespace.png" | relative_url }}){: .inline-image }    

---

# Control Laws

(Bryan Kim)

**Reference Equations**    

The key consideration of CU-D2 is to not let angle  between the horizontal and the linkage exceed a critical value, otherwise it will lose balance and become unstable. This happens because the body has some inertia, causing a lag between when the wheel starts rolling and when the body moves along with it. This critical angle can be calculated at the point where the center of mass of the body’s vertical component falls under a point where it becomes unsupported by the wheel (i.e. the point is to the left or right of the wheel). So critical = arcsin(r1/l), since the radius of the wheel and the length of the linkage are known constants.    
 
**Control Diagram**    

![Photo of control diagram]({{ "/assets/images/statespace.png" | relative_url }}){: .inline-image }    


---

# Computer Simulation in Mujoco

(Alan Muschy)

The robot was modeled with a rectangular prism body, 2 double linkage legs, and wheels attached at the end of each leg. Originally, the model had a constant density, but it was too difficult to balance the robot because the mass of the prism caused the robot to fall too quickly. To fix this, the mass of each wheel was set to ~ 50 grams, the mass of the legs were ~ 10 grams, and the body had a mass of ~ 1 gram. This is unrealistic, but made the balance simulation much easier. The wheels were modeled as motor joints (so an actuation torque could be applied).

**Images of Robot in Virtual Environment**    

![Photo of robot]({{ "/assets/images/mujocorobot.png" | relative_url }}){: .inline-image }    

The robot uses a PD control law:

![Photo of control law]({{ "/assets/images/ctrl.png" | relative_url }}){: .inline-image }    

The robot balance was achieved by tweaking the gain values. A video description of the robot in the virtual environment can be seen below.


<div class="video-container">
  <iframe src="https://youtu.be/kZ9yXSptH3A"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen>
  </iframe>
</div>

The scripts can be found on https://github.com/MagnetMan103/biped_control
And the README file describes how to run the simulations.

---

# Final Report

Here is the final report link for our project:

[Google Doc Final Report](https://docs.google.com/document/d/16YKmQXWkCUaZCsyFVtrkGX-s4uZWguYlUaAa5Dme7Po/edit?tab=t.0)

---
