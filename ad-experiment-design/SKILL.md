---
name: ad-experiment-design
description: "Design and analyze experiments for autonomous driving safety research: simulation studies, closed-course tests, naturalistic driving analysis, and benchmark evaluations. Covers CARLA, SUMO, CommonRoad, nuScenes, Waymo Open, and standard AD evaluation protocols. Use when planning experiments, choosing benchmarks, or designing test matrices for AD safety validation."
metadata:
  version: "1.0.0"
  last_updated: "2026-03-25"
  author: "AD Safety Research Skills"
  tags: ["autonomous-driving", "experiment-design", "simulation", "testing", "benchmarks"]
---

# AD Experiment Design

You are an expert in designing rigorous experiments for autonomous driving safety research, with deep knowledge of simulation platforms, real-world datasets, testing methodologies, and statistical analysis.

## Trigger Conditions

Activate when the user:
- Needs to design experiments for an AD safety paper
- Wants to choose appropriate benchmarks and datasets
- Asks about simulation setup for AD research
- Needs to design a test matrix for scenario-based validation
- Wants to analyze experimental results with proper statistical methods
- Asks about sim-to-real transfer or real-world testing

## Simulation Platforms

### CARLA
- **Type**: Open-source AD simulator (Unreal Engine)
- **Strengths**: Photorealistic rendering, sensor simulation, multi-agent, ScenarioRunner
- **Weather/lighting**: Rain, fog, sun angle, wet roads, night
- **Sensors**: Camera, LiDAR, RADAR, GNSS, IMU — configurable noise models
- **Traffic**: Intelligent traffic manager, custom scenarios via OpenSCENARIO
- **AD stacks**: Integration with Autoware, Apollo, custom agents
- **Benchmarks**: CARLA Leaderboard (driving score, infraction score, route completion)
- **Use case**: End-to-end driving, perception, planning, multi-agent interaction

### SUMO (Simulation of Urban Mobility)
- **Type**: Microscopic traffic simulator
- **Strengths**: Large-scale traffic, signal control, V2X, multi-modal transport
- **Use case**: Traffic flow, V2X safety, intersection analysis, signal optimization
- **Integration**: TraCI (online control), CARLA-SUMO co-simulation

### CommonRoad
- **Type**: Motion planning benchmark suite
- **Strengths**: Standardized scenario format, formal specifications, optimization-based planning
- **Datasets**: 30,000+ scenarios from real traffic data
- **Use case**: Motion planning evaluation, formal safety verification, reachability analysis

### LGSVL / SVL Simulator
- **Type**: AD simulator (Unity-based)
- **Status**: Note — check current availability (was open-sourced, development status varies)
- **Strengths**: Apollo/Autoware integration, sensor simulation

### NVIDIA DRIVE Sim / Isaac Sim
- **Type**: High-fidelity AD simulation (Omniverse)
- **Strengths**: Physically accurate sensors, synthetic data generation, digital twin
- **Use case**: Sensor simulation, synthetic data, hardware-in-the-loop

### Other Simulation Tools
- **IPG CarMaker**: Vehicle dynamics, ADAS testing (commercial)
- **dSPACE ASM**: Real-time vehicle simulation, HIL testing
- **Prescan**: Sensor-realistic simulation (Siemens)
- **VTD (Vires)**: Scenario simulation for ADAS/AD
- **AirSim**: Drone + vehicle simulation (Microsoft, archived)

## Real-World Datasets

### Perception Benchmarks

| Dataset | Modality | Scale | Key Metrics | Best For |
|---|---|---|---|---|
| **nuScenes** | Camera(6)+LiDAR+RADAR | 1000 scenes, 1.4M frames | mAP, NDS | 3D detection, tracking, prediction |
| **Waymo Open** | Camera(5)+LiDAR | 1150 scenes, 12M frames | APH, mAPH, soft-mAP | 3D detection, motion forecasting |
| **KITTI** | Camera(2)+LiDAR | 7481 training frames | AP (3D, BEV) | Stereo, flow, detection (legacy) |
| **Argoverse 2** | Camera(7)+LiDAR | 1000 scenes | CDS (Composite Detection Score) | Detection, forecasting, maps |
| **Lyft L5** | Camera(7)+LiDAR(3) | 170K scenes | mAP | Prediction, detection |
| **ONCE** | Camera+LiDAR | 1M scenes (144K labeled) | mAP | Semi-supervised 3D detection |
| **ZOD** | Camera+LiDAR | 100K frames | mAP | European driving, diverse weather |

### Prediction Benchmarks

| Dataset | Focus | Scale | Key Metrics |
|---|---|---|---|
| **Argoverse 2 Motion** | Vehicle motion forecasting | 250K scenarios | minADE, minFDE, MR, brier-minFDE |
| **Waymo Motion** | Multi-agent forecasting | 570K scenes | soft-mAP, minADE, MR |
| **nuScenes Prediction** | Vehicle trajectory prediction | 1000 scenes | minADE5, minFDE5, MR5 |
| **INTERACTION** | Interactive driving behavior | 40K+ cases | minADE, minFDE |
| **inD / rounD / highD** | Drone-view trajectory data | Hours of recording | minADE, minFDE |

### Planning / End-to-End Benchmarks

| Benchmark | Type | Key Metrics |
|---|---|---|
| **CARLA Leaderboard** | Closed-loop driving | Driving Score, Route Completion, Infraction Score |
| **nuPlan** | Open/closed-loop planning | OLS (Open-Loop Score), CLS (Closed-Loop Score) |
| **CommonRoad** | Motion planning | Cost, feasibility, safety margin |
| **NAVSIM** | Vision-based navigation | PDMS (Predictive Driver Model Score) |

### Safety-Specific Datasets

| Dataset | Focus |
|---|---|
| **FARS** (NHTSA) | Fatal crash records, US national |
| **CRSS** (NHTSA) | Nationally representative crash sample |
| **SCI / CISS** (NHTSA) | Detailed crash investigation |
| **GIDAS** (Germany) | In-depth accident investigation |
| **Euro NCAP test results** | ADAS performance ratings |
| **SHRP2 NDS** | Naturalistic driving study, 3500 drivers |
| **100-Car NDS** | Early naturalistic driving study |
| **DADA-2000** | Driver attention in driving accident scenarios |
| **DoTA** | Detection of Traffic Anomaly |

### Robustness / Corruption Benchmarks

| Benchmark | Tests |
|---|---|
| **nuScenes-C** | Corrupted nuScenes (weather, noise, blur) |
| **KITTI-C** | Corrupted KITTI |
| **Robo3D** | 3D robustness benchmark |
| **3D-OOD** | Out-of-distribution 3D detection |

## Experiment Design Framework

### Phase 1: Research Question → Experimental Variables

```
Independent Variables (IV):
  - Method variants (proposed vs baselines vs ablations)
  - Environmental conditions (weather, lighting, traffic density)
  - Scenario parameters (speed, distance, timing)
  - System configuration (sensor suite, compute budget)

Dependent Variables (DV):
  - Safety metrics (collision rate, TTC, DRAC)
  - Performance metrics (mAP, ADE, driving score)
  - Efficiency metrics (inference time, compute cost)
  - Robustness metrics (performance degradation under perturbation)

Control Variables:
  - Dataset split (train/val/test)
  - Random seeds (report mean ± std over N seeds)
  - Hardware/software versions
  - Hyperparameters (fix across comparisons)
```

### Phase 2: Test Matrix Design

For scenario-based experiments, use a structured test matrix:

```
Scenario Parameters:
  - Ego speed: {30, 50, 70, 90, 110} km/h
  - Target distance: {10, 20, 30, 50, 80} m
  - Lateral offset: {0, 0.5, 1.0, 1.5} m
  - Weather: {clear, light_rain, heavy_rain, fog, night}

Coverage strategy:
  - Full factorial: All combinations (expensive but thorough)
  - Latin hypercube: Efficient sampling of parameter space
  - Boundary value: Focus on ODD boundaries
  - Risk-based: Prioritize high-criticality combinations
```

### Phase 3: Statistical Analysis

**For benchmark comparisons:**
- Report mean and standard deviation over multiple seeds (min 3, prefer 5)
- Use paired t-test or Wilcoxon signed-rank for pairwise comparison
- For multiple comparisons: Bonferroni correction or Tukey HSD
- Report effect size (Cohen's d) for practical significance
- Report confidence intervals (95% CI)

**For safety claims:**
- Statistical testing for rare events: Importance sampling, extreme value theory
- Exposure-based metrics: events per million miles/km driven
- Confidence bounds: How many miles needed to demonstrate X safety level?
  - Rule of thumb (Kalra & Paddock, 2016): ~275 million miles for fatality rate claim
  - Scenario-based approaches can reduce this significantly

**For ablation studies:**
- One variable at a time (OVAT) for clarity
- Report relative and absolute performance changes
- Statistical significance for each ablation

### Phase 4: Reproducibility Checklist

```
□ Random seeds specified and varied
□ Hardware specified (GPU type, count, memory)
□ Software versions listed (PyTorch, CUDA, framework versions)
□ Training details (epochs, learning rate schedule, batch size, optimizer)
□ Dataset version and split specified
□ Pre-processing pipeline documented
□ Inference time measured (batch size, hardware, with/without post-processing)
□ Code availability (GitHub link, license)
□ Model checkpoints available (optional but valued)
```

## Common Experimental Pitfalls

1. **Train/test leakage**: Especially with temporal data — ensure no future information leaks
2. **Cherry-picked scenarios**: Report aggregate metrics, not just best cases
3. **Unfair baselines**: Re-run baselines with same data augmentation and training schedule
4. **Missing failure analysis**: Always show where the method fails
5. **Single seed**: Never report single-run results for stochastic methods
6. **Ignoring latency**: AD requires real-time; report wall-clock inference time
7. **Simulation-only**: Acknowledge sim-to-real gap; discuss transferability
8. **Class imbalance**: AD data is long-tailed; report per-class metrics
9. **Missing confidence intervals**: Critical for safety claims
10. **Overfitting to benchmarks**: Discuss generalization beyond the specific benchmark

## Template: Experiment Section Outline

```
5. Experiments

5.1 Experimental Setup
  - Datasets and benchmarks used (with citations and versions)
  - Evaluation metrics and their motivation
  - Implementation details (architecture, training, hardware)

5.2 Baselines
  - List of compared methods with brief descriptions
  - Fairness statement (same training protocol, data augmentation, etc.)

5.3 Main Results
  - Table: Method × Dataset × Metrics
  - Bold best, underline second-best
  - Statistical significance markers (*, **)

5.4 Ablation Study
  - Component-wise analysis
  - Table: Variant × Metrics

5.5 Analysis
  - Per-scenario / per-condition breakdown
  - Failure case analysis with visualizations
  - Robustness to perturbations
  - Latency analysis

5.6 Real-World Evaluation (if applicable)
  - Closed-course or public road results
  - Sim-to-real transfer analysis
```
