---
title: Abelian sandpile model
summary: Participant in the Dynamics Days LAC 2024, Dynamic Systems Visualization Contest
date: 2023-10-27


authors:
  - gabo

tags:
  - System Dynamics
---

{{< toc mobile_only=true is_open=true >}}

## Overview

Participant in the Dynamics Days LAC 2024, Dynamic Systems Visualization Contest.

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation, FFMpegWriter
import time


# Parameters
Ly = 108
Lx = 192
L = max(Ly, Lx)
factor = 1
pos1 = (Ly // 2, 60)
pos2 = pos1[0] + 1, pos1[1] + 1
pos3 = (int(Ly * 0.75), 165)
pos4 = (int(Ly * 0.26), 140)
pos5 = (int(Ly * 0.85), 115)

simulate_steps = 45_000  # Steps to simulate without recording
record_steps = 4_000  # Steps to record for the animation
N = simulate_steps + record_steps
n = 0
t_init = time.time()

# Initial conditions
z = np.zeros((Ly, Lx), dtype=int)
x, y = np.ogrid[:Ly, :Lx]
mask = (x - pos1[0]) ** 2 + (y - pos1[1]) ** 2 <= (L // 5) ** 2
z[mask] = 4
mask = (x - pos1[0]) ** 2 + (y - pos1[1]) ** 2 <= (L // 6) ** 2
z[mask] = 3
mask = (x - pos1[0]) ** 2 + (y - pos1[1]) ** 2 <= (L // 7) ** 2
z[mask] = 4
mask = (x - pos1[0]) ** 2 + (y - pos1[1]) ** 2 <= (L // 16) ** 2
z[mask] = 3

consecutive_updates = np.zeros((Ly, Lx), dtype=int)
steps_since_last_update = np.zeros((Ly, Lx), dtype=int)
cascade_length = np.zeros(int(np.sqrt(N)), dtype=int)
cascade = 0


def topple(z, consecutive_updates, steps_since_last_update):
    if "critical_cells" not in globals():
        critical_cells = np.zeros_like(z, dtype=bool)

    np.greater_equal(z, 4, out=critical_cells)
    z_aux = np.copy(z)
    z_aux[critical_cells] -= 4
    if "neighbors" not in globals():
        neighbors = np.zeros_like(z)
    neighbors.fill(0)
    neighbors[:-1, :] += critical_cells[1:, :]
    neighbors[1:, :] += critical_cells[:-1, :]
    neighbors[:, :-1] += critical_cells[:, 1:]
    neighbors[:, 1:] += critical_cells[:, :-1]
    z_aux += neighbors
    consecutive_updates[critical_cells] += 1
    consecutive_updates[~critical_cells] = 0
    steps_since_last_update[critical_cells] = 0
    steps_since_last_update[~critical_cells] += 1
    return z_aux, consecutive_updates, steps_since_last_update


def step_simulation():
    global z, consecutive_updates, steps_since_last_update, cascade, cascade_length, n
    cascade += 1
    n += 1
    if n % 100 == 0:
        print(f"Completed {n / N:.2%} in {time.time() - t_init:.2f} seconds")
    no_updates = np.all(consecutive_updates == 0)
    if no_updates or cascade > 9:
        if np.random.rand() < 0.5:
            x, y = pos2
        else:
            x, y = pos1
        z[x, y] += 1
        if cascade < cascade_length.size:
            cascade_length[cascade] += 1
        cascade = 0
    if n % 7 == 0:
        x, y = pos3
        z[x, y] += 1
    if n % 11 == 0:
        x, y = pos4
        z[x, y] += 1
    if n % 29 == 0:
        x, y = pos5
        z[x, y] += 1
    z, consecutive_updates, steps_since_last_update = topple(
        z, consecutive_updates, steps_since_last_update
    )


def update(frame_number, ax, img):
    step_simulation()
    img.set_array(np.log(steps_since_last_update) * 1.1)
    return img


# Pre-simulation
print("Starting pre-simulation...")
for _ in range(simulate_steps):
    step_simulation()
print("Pre-simulation complete.")

# Animation setup
fig, ax = plt.subplots(1, 1, figsize=(Lx, Ly), dpi=10)
img = ax.imshow(
    steps_since_last_update,
    vmin=-1,
    vmax=5,
    cmap="cubehelix_r",
    interpolation="nearest",
)
ax.grid(False)
ax.axis("off")
fig.tight_layout(pad=0)

ani = FuncAnimation(
    fig,
    update,
    fargs=(ax, img),
    interval=1,
    frames=record_steps,
    repeat=False,
)

# Save the animation as a video
output_filename = "sandpile_simulation.mp4"
writer = FFMpegWriter(fps=30, metadata=dict(artist="Gabriel Torre"), bitrate=5000)
ani.save(output_filename, writer=writer, dpi=10)
print(f"Animation saved as {output_filename}")
```