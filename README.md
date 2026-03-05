# TRIM — Trajectory Refinement for Emission Modeling

<div align="center">

![TRIM](https://img.shields.io/badge/TRIM-Trajectory%20Refinement-059669?style=for-the-badge&labelColor=064e3b)
![License](https://img.shields.io/badge/License-MIT-10b981?style=for-the-badge)
![Year](https://img.shields.io/badge/Year-2026-34d399?style=for-the-badge)

**A Simulation-Independent Toolkit for**  
**Vehicle Speed Trajectory Refinement in Emission Modeling**

[🌐 Project Page](https://qinshaomin77.github.io/trim_top) · [🎬 Demo](https://www.youtube.com/watch?v=KDPiDGzVudo) · [⟨/⟩ Code](https://github.com/qinshaomin77/trim_web) · [📋 BibTeX](#citation)

</div>

---

## Overview

**TRIM** (*Trajectory Refinement for emIssion Modeling*) is a simulation-platform-independent, emission-oriented trajectory refinement framework. It bridges the gap between raw microsimulation output and accurate, physically plausible emission estimation.

Traffic microsimulation tools routinely generate vehicle trajectories embedded with non-physical jerk fluctuations, leading to systematic overestimation of vehicle emissions (17–25%). TRIM addresses this by refining speed trajectories through a constrained Mixed-Integer Quadratic Programming (MIQP) optimization that enforces physical realism, car-following safety, and spatial consistency.

---

## Authors

| Name | Affiliation | Profile |
|------|-------------|---------|
| **Shaomin Qin** | Tongji University | [GitHub](https://github.com/qinshaomin77) |
| **Haobing Liu** | Tongji University | [Google Scholar](https://scholar.google.com/citations?user=e-8R2vMAAAAJ&hl=en) |
| **Lishengsa Yue** | Tongji University | [IEEE Xplore](https://ieeexplore.ieee.org/author/37088478561) |

---

## Key Contributions

### 01 · Constrained TRIM Optimization
Incorporates acceleration-dependent jerk envelopes within a tractable MIQP framework to generate physically plausible and car-following-feasible speed trajectories.

### 02 · Reusable TRIM Toolkit
End-to-end pipeline from trajectory refinement to emission estimation and spatial emission-field assessment, deployable across diverse microsimulation platforms.

---

## Methodology

The TRIM framework follows a three-stage workflow:

```
Raw microsimulation          TRIM Optimize              Emission Estimation
trajectories (SUMO FCD)  →  Two-stage MIQP          →  EF lookup & spatial
                             jerk-envelope constrained   emission field mapping
                             refinement
```

### Two-Stage Optimization

| Component | Stage 1: Relaxed | Stage 2: Tightened |
|-----------|------------------|--------------------|
| Objective | min Σ[wᵥ(vₜ−Vₜ)² + wₐ(aₜ)²] | Same |
| Jerk bounds | Constant: −4 ≤ jₜ ≤ 4 | State-dependent: jₘᵢₙ(aₜ) ≤ jₜ ≤ jₘₐₓ(aₜ) |
| Kinematic consistency | Eqs.(3)–(5) | Eqs.(3)–(5) |
| Dynamic bounds | Eqs.(6)–(8) | Eqs.(6)–(8) |
| Car-following safety | Eq.(10) | Eq.(10) |
| Trip-boundary consistency | Eqs.(12)–(13) | Eqs.(12)–(13) |

Stage 1 provides a feasible reference trajectory under constant jerk bounds. Stage 2 is triggered only when the Stage-1 solution violates the acceleration-dependent jerk envelope, refining the trajectory with full state-dependent constraints. A warm-start strategy is adopted between stages to accelerate convergence.

---

## Case Study

Real-world validation on a signalized urban corridor:

| Item | Detail |
|------|--------|
| 📍 Study Area | Jiading North Road, Shanghai |
| 🛸 Data Source | UAV aerial imagery + CV reconstruction |
| 🚗 Car-Following Models | W99 (Wiedemann 99) · Krauss · IDM |
| 📐 Baseline Method | SGS — Savitzky-Golay Smoothing |
| ⚙️ Solver | Gurobi (MIQP) |

---

## Results

TRIM consistently outperforms the SGS baseline across all car-following models and pollutants:

- ✅ NO_x estimation error attenuated to **within 9%**
- ✅ PM₂.₅ estimation error attenuated to **within 7%**
- ✅ Physical jerk envelope compliance across all vehicle types
- ✅ Average optimization time per trajectory: **0.11 s**

---

## Repository Structure

```
trim_top/
├── index.html              # Main project webpage
└── assets/
    ├── img/
    │   ├── TRIM_optimization_framework.png
    │   ├── TwoStage_solution_flow.png
    │   ├── jerk_distribution.png
    │   ├── jerk_envelope.png
    │   ├── speed_accel_distribution.png
    │   ├── Comparison_total_emissions.png
    │   ├── NOx_spatial_emission.png
    │   ├── PM25_spatial_emission.png
    │   └── poster.png
    └── animation/
        ├── trim.mp4
        ├── uav_area.mp4
        └── sumo_area.mp4
```

---

## Acknowledgement

This research is partially supported by:
- National Key R&D Program of China (No. `2023YFB3906900`)
- National Natural Science Foundation of China (No. `52572378`)

---

## Citation

If you use TRIM in your research, please cite:

```bibtex
@article{qin2026trim,
  title   = {TRIM: A Simulation-Independent Toolkit for Vehicle Speed Trajectory Refinement in Emission Modeling},
  author  = {Shaomin Qin and Haobing Liu and Lishengsa Yue},
  year    = {2026},
}
```

---

<div align="center">
  <sub>© 2026 Shaomin Qin · Haobing Liu · Lishengsa Yue · All rights reserved.</sub>
</div>