---
name: ad-behavior-modeling
description: "Behavior modeling for autonomous driving: trajectory prediction (Trajectron++, HiVT, QCNet, MTR), interaction modeling, game-theoretic planning, driver behavior analysis, social force models, and graph-based multi-agent modeling. Use when researching motion forecasting, behavior prediction, interaction-aware planning, or driver behavior analysis for AD safety."
metadata:
  version: "1.0.0"
  last_updated: "2026-03-25"
  author: "AD Safety Research Skills"
  tags: ["autonomous-driving", "behavior-modeling", "trajectory-prediction", "interaction", "game-theory", "motion-forecasting"]
---

# AD Behavior Modeling

You are an expert in behavior modeling for autonomous driving, with deep knowledge of trajectory prediction, multi-agent interaction modeling, game-theoretic planning, driver behavior analysis, and their implications for AD safety.

## Trigger Conditions

Activate when the user:
- Asks about trajectory prediction or motion forecasting methods
- Needs to model multi-agent interactions in driving scenarios
- Wants to understand game-theoretic approaches to planning
- Asks about driver behavior modeling (human drivers or AD agents)
- References social force models, attention mechanisms for interaction, or graph-based modeling
- Needs to evaluate prediction or behavior models for safety

## Trajectory Prediction / Motion Forecasting

### Problem Formulation

```
Input:
  - Agent history: {(x, y, θ, v)_{t-T_obs}, ..., (x, y, θ, v)_t}  for all agents
  - Map context: lane centerlines, boundaries, crosswalks, traffic signals
  - Ego vehicle state and plan (optional)

Output:
  - Future trajectories: {(x, y)_{t+1}, ..., (x, y)_{t+T_pred}}
  - Multi-modal: K possible futures with probabilities
  - Typically: T_obs = 2s (past), T_pred = 6s (future), K = 6 or 12 modes

Challenges:
  - Multi-modality: Multiple plausible futures (turn left vs go straight)
  - Interaction: Agents influence each other's behavior
  - Map compliance: Predictions should follow road topology
  - Long-tail: Rare behaviors (sudden U-turn, jaywalking) are safety-critical
  - Real-time: Must run within planning cycle (10-50ms)
```

### Key Methods

#### Graph-Based / Attention-Based
| Method | Key Innovation | Venue |
|---|---|---|
| **HiVT** (2022) | Hierarchical vector transformer; local + global attention | CVPR 2022 |
| **QCNet** (2023) | Query-centric architecture; relative encoding | CVPR 2023 |
| **MTR** (2023) | Motion transformer with intention queries | NeurIPS 2022 |
| **MTR++** (2024) | Multi-agent multi-timestep refinement | PAMI 2024 |
| **Wayformer** (2023) | Modular attention architecture comparison | ICRA 2023 |
| **GANet** (2023) | Goal area network for trajectory prediction | CVPR 2023 |
| **HPTR** (2024) | Heterogeneous polyline transformer | arXiv |
| **Forecast-MAE** (2023) | Self-supervised pre-training for forecasting | CVPR 2023 |

#### Diffusion / Generative
| Method | Key Innovation | Venue |
|---|---|---|
| **MotionDiffuser** (2023) | Diffusion for multi-agent prediction | CVPR 2023 |
| **Leapfrog Diffusion** (2023) | Efficient diffusion for trajectory prediction | CVPR 2023 |
| **LED** (2023) | Latent diffusion for trajectory prediction | CVPR 2023 |
| **CTG / CTG++** (2023/2024) | Controllable traffic generation via guided diffusion | CoRL/ICML |
| **BehaviorGPT** (2024) | Autoregressive + diffusion hybrid | arXiv |

#### GNN / Social Modeling
| Method | Key Innovation | Venue |
|---|---|---|
| **Trajectron++** (2020) | Graph-structured recurrent model, multi-modal | ECCV 2020 |
| **LaneGCN** (2020) | Lane graph convolution for motion prediction | ECCV 2020 |
| **MANTRA** (2020) | Memory-augmented trajectory prediction | CVPR 2020 |
| **PGP** (2021) | Predictions grounded in graph plans | CoRL 2021 |
| **DenseTNT** (2021) | Dense goal candidates + goal-conditioned prediction | ICCV 2021 |

#### Occupancy / Scene-Level
| Method | Key Innovation | Venue |
|---|---|---|
| **OccFlow** (2023) | Occupancy flow field prediction | arXiv |
| **UniTraj** (2024) | Unified trajectory prediction framework | arXiv |
| **MotionLM** (2023) | Language model for multi-agent motion | ICLR 2024 |
| **Trajeglish** (2024) | Motion prediction as language translation | arXiv |

### Architecture Patterns

```
Pattern 1: Encode-Interact-Decode
  Agent Encoder → Interaction Module → Multi-Modal Decoder
  ├── Agent: PointNet/MLP on trajectory + attributes
  ├── Map: Polyline encoder (VectorNet-style)
  ├── Interaction: Cross-attention / GNN / Transformer
  └── Decoder: GMM / anchor-based / goal-conditioned
  Examples: HiVT, QCNet, MTR

Pattern 2: Query-Based
  Scene Encoding → Learnable Queries → Cross-Attention Decoding
  ├── Queries represent intent hypotheses (turn, lane change, stop)
  ├── Cross-attend to scene features
  └── Refine iteratively (DETR-style)
  Examples: MTR (motion queries), MotionDiffuser

Pattern 3: Generative (Diffusion/Flow)
  Noise + Context → Denoising Network → Trajectory
  ├── Context conditioning via cross-attention
  ├── Multi-agent: joint denoising of all agents
  └── Constraints via guided sampling
  Examples: MotionDiffuser, CTG++, LED

Pattern 4: Autoregressive / Language-Style
  Discretize space-time → Token sequence → Next-token prediction
  ├── Treat trajectory as "sentence"
  ├── Predict next position token autoregressively
  └── Multi-agent: interleaved or parallel decoding
  Examples: MotionLM, Trajeglish, BehaviorGPT
```

---

## Interaction Modeling

### Types of Interactions

```
1. Longitudinal interaction:
   - Car following (Intelligent Driver Model, Wiedemann model)
   - Lead vehicle influence, string stability
   - Safety: rear-end collision risk, TTC analysis

2. Lateral interaction:
   - Lane changing (gap acceptance, courtesy, forced merge)
   - Adjacent lane influence, blocking behavior
   - Safety: sideswipe risk, blind spot conflicts

3. Intersection interaction:
   - Right-of-way negotiation, gap acceptance
   - Turn-taking, yielding behavior
   - Safety: T-bone collision, failed yield

4. Vulnerable road user (VRU) interaction:
   - Pedestrian-vehicle: crosswalk, jaywalking, shared space
   - Cyclist-vehicle: overtaking, right-hook, door zone
   - Safety: highest severity outcomes

5. Multi-agent interaction:
   - Merging with multiple vehicles
   - Roundabout negotiation
   - Unprotected intersection with 3+ agents
```

### Interaction Modeling Approaches

#### Explicit Interaction Models
| Approach | Method | Key Idea |
|---|---|---|
| **Social Force Model** | Helbing & Molnar (1995) | Repulsive/attractive forces between agents |
| **Intelligent Driver Model (IDM)** | Treiber et al. (2000) | Car-following model with desired speed/gap |
| **MOBIL** | Kesting et al. (2007) | Lane change model (politeness factor) |
| **Social GAN** | Gupta et al. (2018) | GAN with social pooling for pedestrian interaction |
| **Social Attention** | Vemula et al. (2018) | Attention mechanism for social interaction |

#### Implicit Interaction (Learned)
| Approach | Method | Key Idea |
|---|---|---|
| **Graph Neural Networks** | Trajectron++, LaneGCN | Agents as nodes, interactions as edges |
| **Cross-Attention** | HiVT, QCNet, MTR | Agents attend to each other's features |
| **Factorized Attention** | Scene Transformer | Marginal + conditional attention layers |
| **Joint Prediction** | MotionLM, Trajeglish | Predict all agents simultaneously |

#### Game-Theoretic Interaction
| Approach | Key Idea | Application |
|---|---|---|
| **Stackelberg Games** | Leader-follower hierarchy | Ego plans considering others' reactions |
| **Nash Equilibrium** | Simultaneous optimization | Multi-agent intersection negotiation |
| **Level-K reasoning** | Bounded rationality hierarchy | Model sophistication levels of drivers |
| **MCTS (Monte Carlo Tree Search)** | Sequential decision under uncertainty | Interactive planning with branching futures |
| **POMDP** | Partially observable decision process | Plan under uncertainty about others' intent |

---

## Game-Theoretic Planning

### Formulations

```
Stackelberg (Leader-Follower):
  Ego (leader): max_{a_ego} J_ego(a_ego, a*_other(a_ego))
  Others (followers): a*_other(a_ego) = argmax_{a_other} J_other(a_ego, a_other)

  Use: Ego plans optimally knowing others will best-respond
  Advantage: Models reactive behavior of other agents
  Challenge: Assumes rationality of other agents

Nash Equilibrium:
  For all agents i: a*_i = argmax_{a_i} J_i(a*_{-i}, a_i)
  No agent can improve by unilaterally changing strategy

  Use: Symmetric multi-agent scenarios (e.g., intersection)
  Advantage: No hierarchy assumption
  Challenge: Multiple equilibria, computation cost, existence

Level-K Reasoning:
  Level 0: Non-strategic (e.g., constant velocity)
  Level 1: Best response to Level 0
  Level 2: Best response to Level 1
  ...

  Use: Model bounded rationality of real drivers
  Advantage: Tractable, captures reasoning depth diversity
  Challenge: Which level to assign to real drivers?
```

### Key Game-Theoretic Methods for AD

| Method | Formulation | Key Innovation | Venue |
|---|---|---|---|
| **ILQR Games** | Dynamic Nash game | Iterative LQR for multi-agent | RSS 2019 |
| **ALGAMES** | Constrained Nash game | Augmented Lagrangian for constrained games | RSS 2020 |
| **CMetric** | Stackelberg + responsibility | Responsibility-aware interaction metric | arXiv |
| **GameFormer** (2023) | Transformer + game-theoretic | Level-K attention for interaction | ICCV 2023 |
| **MARC** (2023) | Multi-agent receding horizon | Cooperative + competitive planning | arXiv |
| **InterPlan** (2024) | Interactive planning via prediction | Joint prediction + planning | arXiv |
| **MPDM** (2015) | Policy-level POMDP | Semantic policy enumeration | IJRR |
| **EPSILON** (2021) | Efficient planning with semantic policy | Semantic-level + trajectory-level | T-RO |

### Responsibility and Blame

```
RSS (Responsibility-Sensitive Safety):
  - Define "proper response" for each agent based on road rules
  - Agent is "responsible" if it deviates from proper response
  - Blame assignment: who caused the safety violation?
  - Limitation: Binary; doesn't capture degree of responsibility

SFF (Safety Force Field):
  - Define safety potential field around each agent
  - Planning must not increase total safety potential
  - Provides continuous safety measure

Research frontier:
  - Counterfactual responsibility: "Would the crash have occurred if agent X had acted differently?"
  - Shapley value-based blame attribution
  - Legal alignment: mapping formal responsibility to traffic law
```

---

## Driver Behavior Analysis

### Human Driver Models

#### Cognitive Models
| Model | Key Concept | Application |
|---|---|---|
| **ACT-R** | Cognitive architecture | Distraction, workload modeling |
| **SOAR** | Rule-based cognitive model | Decision-making in driving |
| **QN-MHP** | Queuing network model of human processing | Multi-task driving behavior |

#### Operational Models
| Model | Level | Key Parameters |
|---|---|---|
| **IDM** (Intelligent Driver Model) | Longitudinal | Desired speed, desired gap, acceleration |
| **Wiedemann** | Longitudinal | Perception thresholds, desired speed |
| **MOBIL** | Lateral | Politeness, acceleration gain threshold |
| **Salvucci & Gray** | Steering | Two-point visual model, near/far points |

#### Data-Driven Driver Behavior
| Approach | Method | Application |
|---|---|---|
| **Driving style clustering** | K-means, GMM on kinematic features | Identify aggressive/normal/timid drivers |
| **HMM driver intent** | Hidden Markov Models | Lane change intent detection |
| **Inverse RL** | Learn reward from demonstrations | Recover driver preferences/objectives |
| **GAIL** | Generative adversarial imitation | Learn realistic driving policies |

### Driver Behavior Metrics

```
Longitudinal:
  - Time headway (THW) distribution
  - Acceleration/deceleration distribution
  - Speed compliance (vs speed limit)
  - Following distance variability

Lateral:
  - Lane keeping accuracy (SDLP — Standard Deviation of Lane Position)
  - Lane change frequency and duration
  - Time-to-lane-crossing (TLC)
  - Steering entropy

Safety-Related:
  - Hard braking frequency (decel > 3.5 m/s²)
  - Near-miss frequency (TTC < threshold)
  - Traffic violation rate
  - Reaction time to events

Driving Style Characterization:
  - Aggressiveness index (composite of speed, acceleration, headway)
  - Fuel efficiency (eco-driving metrics)
  - Risk-taking propensity
  - Adaptation to conditions (weather, traffic)
```

---

## Multi-Agent Simulation

### Traffic Simulation Approaches

| Approach | Realism | Scalability | Controllability |
|---|---|---|---|
| **Rule-based** (IDM + MOBIL) | Low-Medium | Very High | High (interpretable params) |
| **Replay** (log replay) | Perfect (recorded) | Low (fixed scenarios) | None |
| **Imitation learning** | High | Medium | Low |
| **RL-based** (learned agents) | Medium-High | Medium | Medium |
| **Generative** (diffusion/flow) | High | Medium | High (guided sampling) |
| **Game-theoretic** | Medium | Low | High |

### Key Multi-Agent Simulation Methods

| Method | Approach | Key Innovation | Venue |
|---|---|---|---|
| **TrafficSim** (2021) | Imitation learning | Multi-agent traffic simulation | CVPR 2021 |
| **BITS** (2022) | Bi-level imitation | Scalable multi-agent sim | NeurIPS 2022 |
| **TrafficBots** (2023) | World model agents | Multi-agent via world model | ICRA 2023 |
| **CTG++** (2024) | Guided diffusion | Scene-level constrained generation | ICML 2024 |
| **ScenarioNet** (2024) | Scenario library | Large-scale scenario database | NeurIPS 2024 |
| **Waymax** (2023) | JAX simulator | Differentiable multi-agent sim | NeurIPS 2023 |
| **ProSim** (2024) | Prompted simulation | LLM-guided agent behavior | arXiv |
| **LCTGen** (2024) | LLM-controlled generation | Language-described traffic scenarios | NeurIPS 2024 |

---

## Evaluation

### Prediction Metrics

| Metric | Formula / Description | Notes |
|---|---|---|
| **minADE_K** | min over K modes of average L2 error | Standard; K=5 or K=6 |
| **minFDE_K** | min over K modes of final L2 error | Endpoint accuracy |
| **MR_K** (Miss Rate) | % where best mode endpoint > threshold | Typically 2m threshold |
| **brier-minFDE** | minFDE + (1-p_best)² | Penalizes overconfident wrong modes |
| **Soft-mAP** | Area under precision-recall for trajectories | Waymo leaderboard metric |
| **NLL** | Negative log-likelihood of GT under predicted distribution | Probabilistic calibration |
| **CR** (Collision Rate) | % of predicted trajectories that collide | Safety metric |
| **Off-road Rate** | % of predictions leaving drivable area | Feasibility metric |
| **Overlap Rate** | % of multi-agent predictions that overlap | Multi-agent consistency |

### Interaction-Specific Metrics

| Metric | Measures |
|---|---|
| **Scene-level consistency** | Do jointly predicted agents interact realistically? |
| **Conditional prediction accuracy** | Accuracy of agent B given agent A's behavior |
| **Reaction modeling** | Does prediction capture reactive behavior? |
| **Marginal vs joint** | Compare marginal prediction vs joint multi-agent prediction |
| **Causal influence** | Does removing an agent change other predictions correctly? |

### Benchmarks

| Benchmark | Focus | Metrics |
|---|---|---|
| **Argoverse 2 Motion** | Vehicle forecasting | brier-minFDE, minADE, MR |
| **Waymo Open Motion** | Multi-agent forecasting | Soft-mAP, minADE, MR |
| **nuScenes Prediction** | Vehicle prediction | minADE5, minFDE5, MR5 |
| **INTERACTION** | Interactive driving | minADE, minFDE |
| **inD / rounD / highD / exiD** | Drone-view behavior data | minADE, minFDE, NLL |
| **Waymo Sim Agents** | Multi-agent simulation realism | Realism meta-metric |
| **nuPlan** | Planning (includes prediction) | OLS, CLS |

---

## Safety Connections

### Prediction Failures → Safety Impact

```
Failure Mode                    → Safety Consequence
False negative (miss agent)     → Collision with undetected road user
Mode collapse (single mode)     → Miss alternative future → surprise
Over-confidence                 → Insufficient safety margin
Under-confidence                → Overly conservative, traffic disruption
Interaction blindness           → Miss coupled behaviors (yield chains)
Long-tail miss                  → Fail on rare but critical behaviors
Delayed prediction              → Insufficient reaction time
```

### Safe Prediction Requirements
1. **Calibrated uncertainty**: Predicted confidence should match empirical accuracy
2. **Worst-case awareness**: Consider dangerous modes even if low probability
3. **Interaction consistency**: Multi-agent predictions should be jointly feasible
4. **Map compliance**: Predictions should respect road topology (mostly)
5. **Temporal consistency**: Predictions should not oscillate wildly between frames
6. **Fail-safe behavior**: When uncertain, prediction should trigger conservative planning
7. **Real-time**: Prediction must complete within planning cycle budget

### From Prediction to Planning Safety

```
Conservative approach:
  Plan against worst-case prediction (reachability analysis)
  Pro: Safety guarantee | Con: Overly cautious

Probabilistic approach:
  Plan against full distribution of predicted futures
  Risk = Σ P(mode_i) × Cost(collision with mode_i)
  Pro: Balanced | Con: Risk threshold selection

Contingency planning:
  Generate multiple plans for different prediction outcomes
  Switch plans as predictions update
  Pro: Reactive | Con: Computational cost

Interactive planning:
  Jointly optimize ego plan + predicted agent reactions
  Pro: Captures influence | Con: Assumption about rationality
```
