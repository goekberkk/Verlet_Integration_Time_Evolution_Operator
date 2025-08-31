# 1D Three-Mass / Two-Spring Simulation (Verlet & Time-Evolution)

This repository contains a Python simulation of three 1D masses connected in series by two springs with elastic collisions between the masses. It offers two numerical schemes:

- **Verlet integration** (Velocity-Verlet)
- **Time-Evolution Operator** using the matrix exponential `expm` for the linear spring–mass part, plus collision handling

The script also **animates** the motion using `matplotlib.animation` and can render inline in notebooks.

---

## ✨ Features

- Three masses with different **masses (`B`)** and **diameters (`D`)**
- Two springs with configurable **spring constants (`S`)** and **rest length (`L`)**
- **Elastic 1D collisions** that respect diameters (no interpenetration)
- Two integrators: **Verlet** and **Time-Evolution Operator (matrix exponential)**
- Live **position–time plots** + **animation** (inline HTML in notebooks)

---

## 🧱 Requirements

- Python 3.9+
pip install numpy matplotlib scipy ipython
python main.py              # or whatever you named the file

## At launch you’ll be prompted


- `1` → Run Verlet  
- `2` → Run Time-Evolution Operator  
- `0` → Run both (plots + animation for each)

Initial positions and velocities are randomized (positions ordered `x1 < x2 < x3`).  
Simulation runs from `t = 0` to `20 s` with `dt = 0.01`.

## For reproducible runs

``` python
import random as rnd, numpy as np
rnd.seed(42); np.random.seed(42)
```

## ⚙️ Key Parameters (edit at top of file)

| Name            | Meaning                                        | Default            |
|-----------------|------------------------------------------------|--------------------|
| `k`             | reference spring constant                       | 1.0                |
| `m`             | reference mass                                  | 1.0                |
| `L`             | natural spring length (surface-to-surface)      | 5.0                |
| `dt`            | timestep (s)                                    | 0.01               |
| `R`             | reference radius (used to define diameters)     | 1.0                |
| `B=[M1,M2,M3]`  | masses                                          | `[1*m, 3*m, 2*m]`  |
| `S=[S1,S2]`     | spring constants                                | `[1*k, 2*k]`       |
| `D=[R1,R2,R3]`  | diameters via radii                             | `[1*R, 3*R, 2*R]`  |

### Collisions

- Contact checks use `x_left + D_left/2` vs. `x_right - D_right/2`.
- On contact, post-impact velocities are computed by elastic 1D formulas and positions are corrected to remove overlap.

---

## 🎞️ Animation

The function `make_animation(...)` shows an inline HTML animation (Jupyter/VS Code).  
To save a GIF instead, replace the final display lines with:

```python
from matplotlib.animation import PillowWriter
ani.save("simulation.gif", writer=PillowWriter(fps=60))
```
## Tips & Extensions

- **Deterministic ICs:** Set `x1init, x2init, x3init, v1init, v2init, v3init` manually.
- **Performance:** Increase `dt` (with care) or shorten total time to speed up rendering.
- **OS note:** The script calls `os.system('cls')` (Windows). On Linux/macOS use `os.system('clear')`.
- **Further study:** Log energies (kinetic + spring potential) to compare integrators.
