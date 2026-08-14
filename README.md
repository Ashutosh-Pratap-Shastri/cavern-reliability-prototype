# Cavern Reliability Prototype

**Live demo:** https://ashutosh-pratap-shastri.github.io/cavern-reliability-prototype/

A small Monte Carlo reliability-analysis tool for lined rock caverns used to store hydrogen gas at high pressure. Built as a working sketch of the kind of risk-based design tool described in Aalto University's *Safety of High-Pressure Storage Caverns in Finnish Bedrock* (KORVA, 2026–2030) project.

Runs entirely client-side: Python (NumPy) executes in the browser via [Pyodide](https://pyodide.org/), so the whole app is a single static `index.html` with no backend.

## What it models

A cavern is an excavated cavity in hard rock, hundreds of metres underground, lined to hold hydrogen at 250–700 bar. Two things can go wrong:

1. **Wall crushing / tension** — internal gas pressure plus the anisotropic in-situ rock stress concentrate at the cavern boundary. The tool uses the closed-form **Kirsch solution** for a circular opening in a biaxial far-field stress field with internal pressure to get hoop stress at the crown (roof) and sidewalls, and compares it against rock mass strength (intact UCS reduced by GSI via a Hoek–Brown-derived factor) and a crude tensile strength estimate.

2. **Hydraulic jacking** — if the pressure gets too close to the natural stress holding the rock closed above the cavern, gas can literally lift and fracture the rock, causing leaks. Modeled with the classical minimum-principal-stress cover criterion (σ_min ≥ F · P_internal).

All inputs (depth, unit weight, stress ratio k₀, UCS, GSI, pressure) are sampled as independent normal variates from user-supplied mean/COV%, run through both limit states N times, and reduced to a probability of failure and reliability index (β) for each limit state and for the system.

## Explicit assumptions & limitations

- Independent normal variates — no spatial correlation between parameters
- Circular cavern cross-section only (real caverns are often egg- or arch-shaped; shape optimization is a stated open question in the KORVA brief, not modeled here)
- Elastic closed-form (Kirsch) stress solution — no plasticity, no liner interaction, no time-dependent creep
- Rock mass strength via a simplified Hoek–Brown reduction, not a full generalized Hoek–Brown envelope
- No cyclic/fatigue degradation of strength under repeated pressurization — the liner and rock's cyclic behavior is one of the open questions this project exists to answer, and a natural next extension here
- Tensile strength is a rough fraction of UCS, not a measured input

This is an educational prototype, not a design tool. It should be benchmarked against numerical models (FEM/FDM/DEM) and in-situ data before any real use — which is exactly the validation gap projects like KORVA are meant to close.

## Stack

- Pure HTML/CSS/JS shell
- [Pyodide](https://pyodide.org/) (v0.26.1) running NumPy in-browser via WebAssembly
- No build step, no server — deployed as a static site via GitHub Pages

## Possible extensions

- Cavern shape parametrization (circular vs. arched-roof vs. egg-shaped) and depth optimization
- Cyclic loading / fatigue limit state for the liner
- Correlated input sampling (e.g. UCS–GSI correlation)
- Comparison against a numerical (FEM) solution for validation
