---
layout: project
title: Quasi 1D Rocket Nozzle Solver
description: Building a rocket nozzle solver for diverging section
technologies: [Python, Matplotlib, Rocket Nozzles, Quasi1D Simulation]
image: /assets/images/radio-machine-cad.jpg
---

In late October, I decided to solidify my skills in fluid mechanics and subsequently begin my journey in Computational Fluid Dynamics by building a simple rocket nozzle solver giving pressure and velocity graphs.

My main idea was to use the isentropic mach equations given a rocket nozzle envelope. Specifically, I wanted to use the Area Mach Relation by revolving the nozzle envelope around an axis and using the area to compute X position at a certain area ratio. 

This project was very fun for me because it was an introduction to computational fluid mechanics. The program assumes mean flow, constant entropy, and strictly supersonic conditions, with momentum and mass fluctuations varying along the x-direction. While this approach collapses spatial information onto a single axis and therefore loses some detail, rocket nozzle boundary-layer effects are often negligible; this makes the pressure and velocity approximations reasonably accurate—typically within a few percent of a full 3D Navier–Stokes rocket nozzle simulation.

After completing this project, my next goal is to code a X-Foil like software, named Ryfoil, using a more complicated fluid mechanical approach involving vortex panel solutions in uniform flow.


Here is the single-file code for my rocket nozzle simulation:
```python
    import matplotlib.pyplot as plt
import numpy as np
from matplotlib.widgets import Slider
import math 



"""
Initialize plot with 2d nozzle specifications

"""

"""
Physical CSTS
"""

gamma = 1.4

def divergingradius(rt, re, length, z):
    s = z / length
    return rt + (re - rt) * (2*s - s**2)

def iteratedata(rt,re,length,n):
    radii = []
    zloc = np.linspace(0,length,n,endpoint=True)
    for zpoints in np.linspace(0, length, n, endpoint = True):
        radii.append(divergingradius(rt,re,length,zpoints))
    
    return zloc,radii



def getarearatio(rt,re,length,n):
    zloc,radii = iteratedata(rt, re, length, n)
    
    area_list = [math.pi * r**2 for r in radii]

    
    
    # Throat area
    throat_area = min(area_list)
    
    # Compute area ratio 
    area_ratios = [A / throat_area for A in area_list]
    
    return area_ratios, zloc



def numericallysolvemach(rt, re, length, n, tol=1e-6, max_iter=100):
    """
    Given area ratios, z location in nozzle, determine mach number corresponding

    """

    
    area_ratios, zloc = getarearatio(rt, re, length, n)
    M_list = []

    
def getpressureprofile(rt, re, length, n, P0 = 100, tol=1e-6, max_iter=100):
    """
    Computes static pressure along the nozzle using Mach number and isentropic relations.
    
    """
    

    M_list, zloc = numericallysolvemach(rt, re, length, n, tol, max_iter)

    pressures = []
    for M in M_list:
        # Isentropic relation for static pressure
        pressure_ratio = (1 + (gamma - 1) / 2 * M**2) ** (-gamma / (gamma - 1))
        P = P0 * pressure_ratio
        pressures.append(P)
        

    return zloc, pressures


def area_mach_relation(M, area_ratio, gamma=1.4):
    return (1/M) * ((2/(gamma+1))*(1 + (gamma-1)/2 * M**2))**((gamma+1)/(2*(gamma-1))) - area_ratio

def plotter(rt_init, re_init, length, n, P0_init):

    zloc, radii = iteratedata(rt_init, re_init, length, n)
    radii = np.array(radii)
    zloc_p, pressures = getpressureprofile(rt_init, re_init, length, n,
                                           P0=P0_init, tol=1e-6, max_iter=100)

    fig, axs = plt.subplots(1, 2, figsize=(12, 5))
    plt.subplots_adjust(bottom=0.25)  

    # nozzle shape 
    line_top, = axs[0].plot(zloc, radii, color='blue')
    line_bot, = axs[0].plot(zloc, -radii, color='blue')
    axs[0].set_title("Diverging Nozzle Shape")
    axs[0].set_xlabel("Axial Position z (m)")
    axs[0].set_ylabel("Radius r (m)")

    # pressure
    line_P, = axs[1].plot(zloc_p, pressures, color='red')
    axs[1].set_title("Static Pressure Along Nozzle")
    axs[1].set_xlabel("Axial Position z (m)")
    axs[1].set_ylabel("Pressure P (Pa)")

    #slider axes
    ax_re  = fig.add_axes([0.25, 0.12, 0.5, 0.03])
    ax_P0  = fig.add_axes([0.25, 0.06, 0.5, 0.03])

    slider_re = Slider(ax_re, "Expansion Ratio Ae/A*",  rt_init*1.2, re_init*3, valinit=re_init)
    slider_P0 = Slider(ax_P0, "Stagnation Pressure P0",  10, 5*P0_init, valinit=P0_init)

    def update(val):
        re = slider_re.val
        P0 = slider_P0.val

        zloc, radii = iteratedata(rt_init, re, length, n)
        radii = np.array(radii)
        _, pressures = getpressureprofile(rt_init, re, length, n,
                                          P0=P0, tol=1e-6, max_iter=100)
        print(pressures)
        # update nozzle 
        line_top.set_xdata(zloc)
        line_top.set_ydata(radii)
        line_bot.set_xdata(zloc)
        line_bot.set_ydata(-radii)

        # update pressure 
        line_P.set_xdata(zloc)
        line_P.set_ydata(pressures)

        # rescale axes
        for ax in axs:
            ax.relim()
            ax.autoscale_view()

        fig.canvas.draw_idle()

    slider_re.on_changed(update)
    slider_P0.on_changed(update)

    plt.show()


plotter(rt_init=1.0, re_init=4.0, length=2.0, n=50, P0_init=100000)
    
```
