# particle-simulation

A 2D particle simulation built with **Python** and **Pygame**, featuring elastic collisions between particles and an "infection" mechanic where color spreads between particles on contact. The project also includes live plotting of particle populations over time via **Matplotlib**.

The main focus of this project was exploring and comparing different **collision detection algorithms** for large numbers of particles — from a naive brute-force approach to a much faster spatial partitioning approach.

<img width="1918" height="992" alt="image" src="https://github.com/user-attachments/assets/e80290c9-b42f-48be-8a1e-9df11a9739e3" />

## Analysis

This plot tracks the population of each of the 1000 unique particle colors over the course of a simulation run. Since colors spread through collisions ("infection"), the population curves reveal how a small number of colors can rapidly outcompete the rest — most colors stay near zero, while a handful snowball into dominant clusters once they reach a critical mass of particles. The result is a chaotic but clearly non-random growth pattern, driven purely by collision dynamics rather than any built-in bias toward specific colors.

<img width="1148" height="822" alt="image" src="https://github.com/user-attachments/assets/ca7d49b8-eaf2-4739-aaa7-a9f35eacf560" />

## Particle Simulation With Center Mass Point 

A variant of the simulation (`centered_gravitity_point.py`) where a fixed central mass point exerts a gravitational pull on all surrounding particles. Instead of bouncing freely off walls, particles are drawn into orbit-like trajectories around the center, gradually forming ring- and spiral-shaped structures as they settle into a dynamic equilibrium between gravitational attraction and their own momentum.

<img width="720" height="405" alt="output" src="https://github.com/user-attachments/assets/cc387505-a5bd-4277-b3e7-03e9a2a4d241" />

## Features

- Circular particles with velocity-based movement and wall bounce collisions
- Elastic pairwise collision resolution (overlap correction + velocity swap)
- "Infection" spreading mechanic: particles can pass their color/state to particles they collide with
- Randomized particle colors ("color chaos" mode)
- Simple start menu (press `SPACE` to begin the simulation)
- Live population-over-time plot per color, shown after the simulation window is closed

## Collision Algorithms

I tried implementing the particle collision detection with two different algorithms to compare performance:

- **`slow_algorithm.py`** — a brute-force **O(n²)** approach that checks every particle against every other particle each frame. Simple to implement, but performance drops quickly as the particle count grows.
- **`spatial_partitioning_alogrithm.py`** — the same simulation logic, but collisions are checked using **spatial partitioning** (a uniform grid). Particles are bucketed into grid cells based on position, and each particle is only checked against particles in its own cell and neighboring cells. This drastically reduces the number of collision checks per frame and allows the simulation to comfortably handle far more particles (tested with ~1000 particles) at higher resolution.

Comparing these two implementations side by side was a good hands-on way to see the real-world impact of algorithmic complexity on a live, frame-by-frame simulation.

## Project Structure

```
Particle_Simulation/
├── Menu/
│   └── main_menu.py                      # Draws the start menu shown before the simulation begins
├── Scripts/
│   ├── slow_algorithm.py                 # Particle simulation using the O(n²) brute-force collision check
│   ├── spatial_partitioning_alogrithm.py # Particle simulation using spatial partitioning for collision detection
│   └── centered_gravitity_point.py       # Variant of the simulation involving a central gravity point
└── README.md
```

| Folder | Contents |
|---|---|
| `Menu/` | Start menu shown before the simulation begins |
| `Scripts/` | Simulation and algorithm implementations |

## Requirements

- Python 3.x
- [pygame](https://www.pygame.org/)
- [numpy](https://numpy.org/)
- [matplotlib](https://matplotlib.org/)

Install dependencies:

```bash
pip install pygame numpy matplotlib
```

## Usage

Run either version of the simulation directly from the repo root:

```bash
python Scripts/slow_algorithm.py
```

or

```bash
python Scripts/spatial_partitioning_alogrithm.py
```

Press **SPACE** at the start menu to begin the simulation. Close the simulation window to end it and view the population plot.

## Notes

This project was primarily a learning exercise in simulation logic, collision physics, and algorithmic optimization — the spatial partitioning version in particular was built to see firsthand how much of a performance difference a smarter data structure can make over a naive nested-loop approach.
