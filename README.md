# AUV-Confidence-Weighting

Innovation-based confidence weighting versus gating and multiple-model filtering for GPS-denied AUV navigation.

## Description

Autonomous underwater vehicles without GPS rely on inertial navigation aided by a Doppler velocity log (DVL), vision, and sonar-based SLAM. The reliability of these sensors changes with water clarity and seabed texture, so fixed measurement-noise models become inconsistent when a sensor degrades.

This repository contains the simulation and estimators for a confidence-driven weighting scheme for Kalman-filter-based AUV position estimation. Each sensor receives a confidence score computed from windowed normalized innovation squared (NIS). The score inflates that sensor's measurement covariance, gradually reducing its influence instead of removing it from the filter.

The method is compared against:

- Chi-square innovation gating
- Sage-Husa-style adaptive filter
- Per-sensor interacting multiple model (IMM) filter
- Hard sensor selection
- Oracle upper-bound references

## Results

Evaluated on 100 Monte Carlo seeds (2001–2100) not used for tuning. Position RMSE in meters (95% CI):

| Method | Scenario A | Scenario B |
|---|---|---|
| Gated | 0.370 ± 0.011 | 0.475 ± 0.006 |
| Hard selector | 0.803 ± 0.424 | 0.932 ± 0.443 |
| **Soft weighting (proposed)** | **0.316 ± 0.006** | **0.445 ± 0.005** |
| Sage-Husa style | 0.326 ± 0.006 | 0.479 ± 0.007 |
| IMM | 0.322 ± 0.006 | 0.425 ± 0.005 |
| Oracle (true noise) | 0.306 ± 0.005 | 0.450 ± 0.005 |
| Oracle (noise + outlier flags) | 0.294 ± 0.005 | 0.420 ± 0.004 |

- Soft weighting lowers RMSE relative to gating by about 15% (Scenario A) and 6% (Scenario B).
- In Scenario A, accuracy is comparable to IMM at about 7.6 times lower runtime.
- IMM is more accurate in Scenario B.
- Hard selection is unstable and can lose track when several sensors are removed together.

## Simulation

- 2D horizontal-plane kinematics, 1000 time steps per run
- Scenario A: sinusoidal heading with separated vision, SLAM, and DVL failures
- Scenario B: racetrack trajectory with overlapping SLAM and DVL failures
- Six-state Kalman filter (position, velocity, acceleration bias)

## Installation

```bash
pip install -r requirements.txt
```

## Usage

```bash
python sim.py
```

## Citation

```bibtex
@inproceedings{durrani2027auv,
  title  = {Innovation-Based Confidence Weighting versus Gating and Multiple-Model Filtering for GPS-Denied AUV Navigation},
  author = {Durrani, Babar Khan},
  year   = {2027}
}
```

## Author

Babar Khan Durrani — Bahria University, Islamabad
GitHub: [BabarKhanD](https://github.com/BabarKhanD)
