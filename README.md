# AD Safety Research Skills

A collection of 10 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills for autonomous driving (AD) safety research, spanning safety standards, scenario analysis, crash data, paper writing, world models, diffusion/3DGS, VLAs, end-to-end driving, and behavior modeling across automotive, AI, and transportation domains.

Built for researchers who work at the intersection of **automotive engineering**, **artificial intelligence**, and **transportation safety**.

## Skills Overview

### Core Research Workflow

| Skill | Description | Highlights |
|-------|-------------|------------|
| [`ad-safety-research`](ad-safety-research/) | Orchestrator that routes to domain-specific sub-skills | 10 core AD safety research areas, cross-domain integration framework, routing logic |
| [`ad-literature-review`](ad-literature-review/) | Deep literature search across AD-specific venues and databases | 30+ venues (IV, ITSC, T-ITS, CVPR WAD, NeurIPS ML4AD, TRB), keyword expansion, 10+ key research groups |
| [`ad-paper-writing`](ad-paper-writing/) | Paper writing for AD safety conferences and journals | Venue-specific conventions (IEEE, CVPR, NeurIPS, TRB, SAE), section guides, metrics tables, rebuttal writing |

### Safety Analysis & Standards

| Skill | Description | Highlights |
|-------|-------------|------------|
| [`ad-scenario-analysis`](ad-scenario-analysis/) | Safety scenario analysis, ODD definition, and hazard analysis | 6-layer scenario model, HARA/STPA/FMEA/FTA methods, ASIL matrix, SOTIF analysis, ODD template, criticality metrics |
| [`ad-standards-navigator`](ad-standards-navigator/) | Navigate AD safety standards and regulatory frameworks | ISO 26262, ISO 21448 (SOTIF), UL 4600, ISO/PAS 8800, SAE J3016, UNECE R157, US/EU/China/Japan regulations, 10 research gaps |

### Data & Experiments

| Skill | Description | Highlights |
|-------|-------------|------------|
| [`ad-experiment-design`](ad-experiment-design/) | Design simulation experiments and benchmark evaluations | CARLA, SUMO, CommonRoad, nuScenes/Waymo/Argoverse datasets, test matrix design, statistical analysis, reproducibility checklist |
| [`ad-dataset-analysis`](ad-dataset-analysis/) | Analyze crash data and compute safety metrics | FARS, CRSS, GIDAS, SHRP2, surrogate safety measures (TTC, DRAC, PET), exposure-based analysis, rare event statistics |

### Emerging Methods

| Skill | Description | Highlights |
|-------|-------------|------------|
| [`ad-generative-models`](ad-generative-models/) | World models, diffusion, flow matching, and 3DGS for AD | GAIA-1, DriveDreamer, GenAD, Vista, UniSim, CTG++, MagicDrive, StreetGaussians, DrivingGaussian, sensor synthesis pipelines |
| [`ad-foundation-models`](ad-foundation-models/) | VLAs, end-to-end driving, and LLM/VLM-based planning | RT-2, OpenVLA, DriveVLM, UniAD, VAD, SparseDrive, GPT-Driver, 4 safety architectures, 6 critical safety challenges, standards gap analysis |
| [`ad-behavior-modeling`](ad-behavior-modeling/) | Trajectory prediction, interaction modeling, and game-theoretic planning | HiVT, QCNet, MTR, Trajectron++, MotionDiffuser, Stackelberg/Nash/Level-K games, IDM/MOBIL, multi-agent simulation, prediction-to-safety connections |

## Target Venues

**Conferences**: IV, ITSC, ICRA, IROS, CVPR, ECCV, NeurIPS, AAAI, CoRL, RSS, TRB, ESV, SAE WCX

**Journals**: T-ITS, T-IV, RA-L, Accident Analysis & Prevention, Safety Science, Vehicle System Dynamics, TRC, TRF, RESS

**Workshops**: CVPR WAD, NeurIPS ML4AD, ECCV ROAD, SafeAI (AAAI)

## Getting Started

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI, desktop app, or IDE extension

### Installation

Clone the repository:

```bash
git clone https://github.com/RoboSafe-Lab/ad-safety-research-skills.git
```

Add individual skill directories to your Claude Code project, or use `ad-safety-research` as the orchestrator entry point. You can install skills via:

```
/install-skill path/to/ad-safety-research-skills/ad-safety-research
```

Or add all skills at once by pointing to the root directory.

### Usage Examples

Ask Claude Code questions like:

- *"Find the latest papers on SOTIF analysis for L4 highway driving"* -> triggers `ad-literature-review`
- *"Write the experiment section for my nuScenes perception robustness paper"* -> triggers `ad-paper-writing`
- *"Perform STPA analysis on the AEB function for pedestrian scenarios"* -> triggers `ad-scenario-analysis`
- *"What does ISO 21448 require for triggering condition identification?"* -> triggers `ad-standards-navigator`
- *"Design a test matrix for evaluating my planner in CARLA with weather variations"* -> triggers `ad-experiment-design`
- *"Analyze FARS data to find the distribution of crash types preventable by ADS"* -> triggers `ad-dataset-analysis`
- *"Compare world model architectures for closed-loop AD simulation"* -> triggers `ad-generative-models`
- *"What are the safety implications of deploying a VLA-based driving system?"* -> triggers `ad-foundation-models`
- *"Survey game-theoretic interaction models for intersection planning"* -> triggers `ad-behavior-modeling`

## Skill Architecture

```
ad-safety-research (orchestrator)
├── Research Workflow
│   ├── ad-literature-review    — find and survey papers
│   └── ad-paper-writing        — write and revise papers
├── Safety Analysis
│   ├── ad-scenario-analysis    — hazard analysis, ODD, scenarios
│   └── ad-standards-navigator  — ISO 26262, SOTIF, UL 4600, regulations
├── Data & Experiments
│   ├── ad-experiment-design    — simulation, benchmarks, test matrices
│   └── ad-dataset-analysis     — crash data, safety metrics, statistics
└── Emerging Methods
    ├── ad-generative-models    — world models, diffusion, 3DGS
    ├── ad-foundation-models    — VLA, E2E driving, LLM planning
    └── ad-behavior-modeling    — prediction, interaction, game theory
```

## Key Topics Covered

<details>
<summary><b>Safety Standards & Regulations</b></summary>

- ISO 26262 (Functional Safety) — ASIL determination, HARA, safety lifecycle
- ISO 21448 (SOTIF) — triggering conditions, functional insufficiencies, Areas 1-4
- UL 4600 — safety case framework for autonomous products
- ISO/PAS 8800 — AI safety for road vehicles
- ISO 34502/34503 — scenario-based evaluation, ODD taxonomy
- SAE J3016 — levels of driving automation (L0-L5)
- UNECE R157 (ALKS) — first international L3 regulation
- Regional regulations: NHTSA, EU GSR/AI Act, China MIIT, Japan, UK

</details>

<details>
<summary><b>Hazard Analysis Methods</b></summary>

- HARA (Hazard Analysis and Risk Assessment) with ASIL matrix
- SOTIF analysis (triggering conditions, functional insufficiencies)
- STPA (Systems-Theoretic Process Analysis)
- FMEA (Failure Mode and Effects Analysis)
- FTA (Fault Tree Analysis)
- 6-layer scenario model (PEGASUS)
- Criticality metrics: TTC, DRAC, PET, CPI, RSS distance

</details>

<details>
<summary><b>Datasets & Benchmarks</b></summary>

- **Perception**: nuScenes, Waymo Open, KITTI, Argoverse 2, ONCE, ZOD
- **Prediction**: Argoverse 2 Motion, Waymo Motion, INTERACTION, inD/rounD/highD
- **Planning**: CARLA Leaderboard, nuPlan, CommonRoad, NAVSIM, Bench2Drive
- **Crash data**: FARS, CRSS, CISS, GIDAS, SHRP2 NDS, Euro NCAP
- **Robustness**: nuScenes-C, KITTI-C, Robo3D

</details>

<details>
<summary><b>Generative Models for AD</b></summary>

- **World models**: GAIA-1, DriveDreamer, GenAD, Vista, UniSim, Copilot4D, OccWorld
- **Diffusion**: CTG++, MagicDrive, MotionDiffuser, LiDARGen, Panacea, DrivingDiffusion
- **Flow matching**: FlowBot, MotionFlow, comparison with diffusion
- **3D Gaussian Splatting**: StreetGaussians, DrivingGaussian, HUGS, PVG, NeuRAD
- **Sensor generation**: Camera, LiDAR, radar synthesis pipelines

</details>

<details>
<summary><b>Foundation Models for AD</b></summary>

- **VLAs**: RT-2, OpenVLA, DriveVLM, LMDrive, DriveLM
- **End-to-end driving**: UniAD, VAD, SparseDrive, Transfuser, TCP, ThinkTwice
- **LLM-based planning**: GPT-Driver, DiLu, LanguageMPC, Agent-Driver
- **Safety challenges**: hallucination, latency, adversarial robustness, V&V gaps
- **Safety architectures**: FM + safety filter, FM as advisor, hierarchical, ensemble

</details>

<details>
<summary><b>Behavior Modeling</b></summary>

- **Trajectory prediction**: HiVT, QCNet, MTR, Trajectron++, MotionDiffuser, MotionLM
- **Interaction modeling**: social force, GNN, cross-attention, joint prediction
- **Game theory**: Stackelberg, Nash equilibrium, Level-K reasoning, ILQR Games
- **Driver behavior**: IDM, MOBIL, inverse RL, GAIL, driving style clustering
- **Multi-agent simulation**: TrafficBots, CTG++, Waymax, BITS, ProSim

</details>

## Contributing

Contributions are welcome! If you'd like to add or improve a skill:

1. Fork this repository
2. Create a new branch for your changes
3. Follow the existing skill format (YAML frontmatter + structured markdown in `SKILL.md`)
4. Submit a pull request with a description of what the skill covers

### Skill File Format

Each skill follows this structure:

```
skill-name/
└── SKILL.md          # Main skill file with YAML frontmatter
```

```yaml
---
name: skill-name
description: "What the skill does and when to trigger it"
metadata:
  version: "1.0.0"
  last_updated: "YYYY-MM-DD"
  tags: ["autonomous-driving", ...]
---

# Skill content in markdown...
```

## License

This project is open source. See [LICENSE](LICENSE) for details.

## Acknowledgments

Built by [RoboSafe Lab](https://github.com/RoboSafe-Lab) for the autonomous driving safety research community.

---

*If you find these skills useful for your research, consider giving this repo a star!*
