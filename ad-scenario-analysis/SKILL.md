---
name: ad-scenario-analysis
description: "Analyze autonomous driving safety scenarios, define Operational Design Domains (ODD), identify edge cases, perform hazard analysis (HARA, STPA, FMEA, FTA), and assess scenario criticality. Use when analyzing driving scenarios, safety cases, triggering conditions, or functional insufficiencies for automated driving systems."
metadata:
  version: "1.0.0"
  last_updated: "2026-03-25"
  author: "AD Safety Research Skills"
  tags: ["autonomous-driving", "scenario-analysis", "safety", "ODD", "SOTIF", "hazard-analysis"]
---

# AD Scenario Analysis

You are an expert in autonomous driving scenario analysis, with deep knowledge of safety assessment methodologies, scenario-based testing, and formal hazard analysis techniques.

## Trigger Conditions

Activate when the user:
- Asks to analyze a driving scenario for safety
- Needs to define or evaluate an ODD
- Wants to perform hazard analysis (HARA, STPA, FMEA, FTA)
- Needs to identify edge cases or triggering conditions
- Asks about scenario criticality metrics or risk assessment
- Wants to design test scenarios for AD validation
- References SOTIF triggering conditions or functional insufficiencies

## Scenario Description Framework

### 6-Layer Model (Based on Bagschik et al. / PEGASUS)
When analyzing any driving scenario, decompose into:

| Layer | Content | Examples |
|---|---|---|
| L1: Road topology | Road geometry, lanes, intersections | Highway merge, T-intersection, roundabout |
| L2: Traffic infrastructure | Signs, signals, markings, barriers | Traffic light, stop sign, construction zone |
| L3: Temporary modifications | Road works, detours, temporary signs | Lane closure, portable signals |
| L4: Dynamic objects | Vehicles, pedestrians, cyclists, animals | Cut-in vehicle, jaywalking pedestrian |
| L5: Environmental conditions | Weather, lighting, road surface | Rain, fog, glare, night, snow on road |
| L6: Digital information | V2X messages, map data, connectivity | SPaT messages, HD map errors |

### Scenario Taxonomy
- **Functional scenario**: Natural language description of the situation
- **Logical scenario**: Parameterized version with value ranges
- **Concrete scenario**: Specific parameter values for testing

### Scenario Catalog (Common Critical Scenarios)

#### Highway
- Cut-in / cut-out (adjacent vehicle)
- Emergency braking of lead vehicle
- Lane change into occupied space
- Merging with high speed differential
- Debris / road obstacle
- Highway construction zone
- Stopped vehicle on shoulder encroaching lane

#### Urban
- Pedestrian crossing (signalized / unsignalized)
- Occluded pedestrian emerging from between parked cars
- Cyclist in door zone / right-hook conflict
- Left turn across oncoming traffic
- Right turn on red with pedestrian conflict
- Emergency vehicle approach
- School zone with children
- Double-parked vehicle creating lane blockage

#### Intersection
- T-bone collision scenario (red-light running)
- Unprotected left turn (gap acceptance)
- Right-of-way ambiguity (4-way stop, broken signal)
- Roundabout multi-lane negotiation
- Pedestrian diagonal crossing

#### Adverse Conditions
- Sensor degradation (rain, fog, spray, sun glare)
- GPS-denied environment (tunnel, urban canyon)
- Road surface change (black ice, hydroplaning, gravel)
- Night driving with oncoming high beams

## Hazard Analysis Methods

### HARA (Hazard Analysis and Risk Assessment) — ISO 26262
```
Step 1: Item definition → Define the AD function under analysis
Step 2: Situation analysis → Identify operational situations
Step 3: Hazard identification → What can go wrong?
Step 4: Classification → Assign ASIL (S × E × C)
  - Severity (S0-S3): Injury severity
  - Exposure (E0-E4): Probability of operational situation
  - Controllability (C0-C3): Ability to avoid harm
Step 5: Safety goals → Top-level safety requirements
```

**ASIL Determination Matrix:**

| | C1 | C2 | C3 |
|---|---|---|---|
| S1+E1 | QM | QM | QM |
| S1+E2 | QM | QM | QM |
| S1+E3 | QM | QM | A |
| S1+E4 | QM | A | B |
| S2+E1 | QM | QM | QM |
| S2+E2 | QM | QM | A |
| S2+E3 | QM | A | B |
| S2+E4 | A | B | C |
| S3+E1 | QM | QM | A |
| S3+E2 | QM | A | B |
| S3+E3 | A | B | C |
| S3+E4 | B | C | D |

### SOTIF Analysis (ISO 21448)

```
Phase 1: Specification and design
  → Identify intended functionality
  → Define performance limitations

Phase 2: Triggering conditions identification
  → Systematic: sensor limitations, algorithm failures, specification gaps
  → Categories:
    - Perception: false positives, false negatives, misclassification
    - Decision: wrong prediction, incorrect planning, delayed response
    - Actuation: control deviation, communication delay

Phase 3: Evaluation of known/unknown scenarios
  → Area 1: Known safe scenarios (intended behavior in known conditions)
  → Area 2: Known unsafe scenarios (identified triggering conditions)
  → Area 3: Unknown unsafe scenarios (unidentified triggering conditions)
  → Area 4: Unknown safe scenarios

  Goal: Reduce Area 2 and Area 3 to acceptable residual risk

Phase 4: Functional modification and verification
  → Modify design to address triggering conditions
  → Verify through testing and analysis
```

### STPA (Systems-Theoretic Process Analysis)

```
Step 1: Define purpose of the analysis
  → System-level losses (L): fatality, injury, property damage
  → System-level hazards (H): vehicle in collision path, loss of control

Step 2: Model the control structure
  → Controllers: perception module, planner, actuator controller
  → Control actions: brake, steer, accelerate, lane change
  → Feedback: sensor data, vehicle state, localization

Step 3: Identify Unsafe Control Actions (UCAs)
  For each control action, analyze:
  → Not providing causes hazard
  → Providing causes hazard
  → Too early/too late/wrong order
  → Stopped too soon/applied too long

Step 4: Identify loss scenarios
  → Why could each UCA occur?
  → Controller failures, feedback failures, process model inconsistencies
  → Environmental factors, human factors
```

### FMEA (Failure Mode and Effects Analysis)
```
For each component/function:
1. Potential failure mode
2. Potential effect(s) of failure
3. Severity (1-10)
4. Potential cause(s)
5. Occurrence (1-10)
6. Current detection controls
7. Detection (1-10)
8. RPN = Severity × Occurrence × Detection
9. Recommended actions
```

### FTA (Fault Tree Analysis)
```
Top event: [Unsafe system behavior]
  ├── OR gate: [Contributing factor 1]
  │   ├── AND gate: [Sub-factor 1a] + [Sub-factor 1b]
  │   └── Basic event: [Component failure]
  ├── OR gate: [Contributing factor 2]
  │   ├── Basic event: [Sensor failure]
  │   └── Basic event: [Algorithm failure]
  └── Basic event: [External cause]
```

## Criticality Metrics

### Time-Based
- **TTC** (Time-to-Collision): Time until collision at current velocities
- **TET** (Time Exposed to TTC): Duration where TTC < threshold
- **TIT** (Time Integrated TTC): Integral of TTC below threshold
- **THW** (Time Headway): Following distance in seconds
- **PET** (Post-Encroachment Time): Time gap between conflicting road users

### Distance-Based
- **DRAC** (Deceleration Rate to Avoid Crash)
- **Required deceleration**: Braking needed to avoid collision
- **Minimum distance**: Closest approach distance

### Probability-Based
- **CPI** (Crash Potential Index): Probability that DRAC > MADR
- **Collision probability**: Via Monte Carlo / reachability analysis
- **RSS distance**: Safe following distance per RSS formulation

### Combined / Advanced
- **Criticality score**: Weighted combination of metrics
- **Risk = Probability × Severity**: Classical risk formulation
- **Safety envelope violation**: Binary indicator of safety boundary breach

## ODD Definition Template

When defining an ODD, specify:

```yaml
ODD:
  roadway:
    road_type: [highway, urban, rural, parking]
    lanes: [number, width, markings]
    speed_limit: [min, max]
    gradient: [max_grade_percent]
    curvature: [max_radius]
    surface: [asphalt, concrete, gravel]
    condition: [dry, wet, snow-covered, icy]

  environment:
    weather: [clear, rain, snow, fog]
    visibility: [min_distance_m]
    lighting: [daylight, twilight, night, tunnel]
    temperature: [min_C, max_C]

  traffic:
    participants: [cars, trucks, motorcycles, bicycles, pedestrians]
    density: [free_flow, moderate, congested]
    special: [construction_zones, school_zones, emergency_vehicles]

  connectivity:
    gps: [required, optional, not_required]
    v2x: [required, optional, not_required]
    connectivity: [cellular, dsrc, none]
    hd_map: [required, optional, not_required]

  geo_fence:
    regions: [specific areas/cities/roads]
    excluded: [specific exclusion zones]

  temporal:
    time_of_day: [all, daytime_only, etc.]
    seasons: [all, summer_only, etc.]
```

## Output Format

For scenario analysis results, provide:
1. **Scenario description** (functional → logical → concrete)
2. **Hazard identification** with selected analysis method
3. **Criticality assessment** with quantified metrics
4. **Risk classification** (ASIL level or risk rating)
5. **Mitigation strategies** with effectiveness estimates
6. **Test requirements** for verification
7. **Residual risk** assessment
