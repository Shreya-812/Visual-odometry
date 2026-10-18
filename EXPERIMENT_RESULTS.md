# EuRoC V1_01_easy Experiment Results

This document records a local reproduction and three single-run experiments using the FALCOdrone Visual Inertial Odometry pipeline. The upstream project is available at <https://github.com/FALCOdrone/Visual_Inertial_Odometry>. The pipeline implementation is upstream work; the results below are from this local setup and should not be presented as a new VIO implementation.

Dataset: EuRoC MAV, Vicon Room 1, V1_01_easy. Each run used the same bag at playback rate 0.5. Metrics are from `evaluate_trajectory.py` and are after the evaluator's SE(3) alignment.

| Run | ESKF position ATE RMSE | ESKF mean rotation error | ESKF 5 m mean translation RPE | VIO position ATE RMSE |
|---|---:|---:|---:|---:|
| Baseline | 2.073 m | 24.197° | 22.836% | 2.514 m |
| Feature tuning (`150` corners, quality `0.1`) | 1.803 m | 31.402° | 24.747% | 1.650 m |
| CLAHE enabled | 0.508 m | 7.632° | 7.453% | 0.536 m |
| VIO NIS gate (`12.592`) | 136.741 m | 128.302° | 497.851% | 1.793 m |

## Interpretation

- CLAHE produced the best metrics in this set of runs.
- Feature tuning reduced position ATE in these runs, while ESKF rotation error and 5 m translation RPE were worse than baseline.
- The NIS gate result was catastrophic for the fused ESKF trajectory; do not use that setting as an improvement.
- These are single runs, not a reproducibility study. Ground-truth sample counts differed between runs, so repeat baseline and CLAHE before making a strong performance claim.

## Attribution

The ROS 2 pipeline, VIO frontend, and ESKF are from the upstream FALCOdrone repository. The local work here consists of running the pipeline in WSL, testing parameter/preprocessing/filter-gating variants, and recording the observed results. Confirm redistribution rights before publishing upstream source files; this repository copy did not contain a top-level `LICENSE` file when checked.
