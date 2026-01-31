# Bump-on-Tail Instability — Single-Mode Vlasov–Poisson Simulation

This repository contains a Python notebook implementing a **reduced single-mode kinetic model** of the **bump-on-tail instability** in an electrostatic plasma. The code is part of an ongoing research project and is intended for **exploratory and diagnostic studies**, rather than as a general-purpose plasma simulation package.

The notebook evolves particles self-consistently with a prescribed Langmuir mode and reproduces key features of the instability, including linear exponential growth, saturation, and resonant velocity-space modification.

------

## Scientific Overview

The bump-on-tail instability arises when an energetic electron beam is superimposed on a background plasma, creating a positive slope in the velocity distribution near the wave phase velocity. This free energy drives unstable Langmuir waves through resonant wave–particle interaction.

Rather than solving the full Vlasov–Poisson system on a spatial grid, this notebook adopts a **single-mode approximation**:

- The electric field is restricted to one Langmuir mode with fixed wavenumber k.
- The field amplitude evolves self-consistently from the particle current projected onto that mode.
- Particles are evolved kinetically using a leapfrog integrator.

This approach retains the essential physics of linear growth, nonlinear trapping, and saturation while remaining computationally lightweight and closely connected to analytic theory.

------

## Model Description

### Plasma Model
- One-dimensional, electrostatic system
- Immobile ion background
- Two electron populations:
  - Background Maxwellian
  - Drifting beam Maxwellian
- Periodic spatial domain of length  
  L = 2π / k

### Field Representation
The electric field is represented as a single cosine mode:
E(x, t) = E(t) cos(k x − ω_p t)
where the amplitude E(t) is evolved self-consistently.

### Particle Dynamics
Particles follow the Vlasov equation:
dx_i/dt = v_i ,   dv_i/dt = −(e / m_e) E(x_i, t)
and are advanced in time using a standard leapfrog scheme.

------

## Numerical Method

- **Leapfrog integration** for particle positions and velocities
- **Direct Fourier projection** of particle velocities to compute the \( k \)-mode current
- Self-consistent update of the electric field amplitude from the projected current
- Periodic boundary conditions in space

The simulation is performed in **normalized units**:
ω_p = 1 ,  e = 1 ,  m_e = 1
allowing direct comparison between numerical growth rates and analytic predictions.

------

## Diagnostics and Analysis

The notebook includes diagnostics for both field-level and particle-level behavior:

### Field Diagnostics
- Time evolution of the field amplitude |E(t)|
- Automated or manual identification of linear growth intervals
- Exponential growth-rate fitting via linear regression of log |E(t)|

### Particle Diagnostics
- Velocity sampling of beam and background populations
- Construction of velocity-space PDFs at different stages:
  - Early linear phase
  - Late linear phase
  - Post-saturation nonlinear phase
- Visualization of resonant flattening via differences in beam velocity distributions

### Theory Comparison
The fitted growth rate is compared against linear kinetic theory by solving the two-Maxwellian dispersion relation for complex frequency roots using the same normalized parameters as the simulation.
