---
name: ad-dataset-analysis
description: "Analyze crash data, naturalistic driving data, and compute safety metrics for autonomous driving research. Covers FARS, CRSS, GIDAS, SHRP2, and fleet telemetry analysis. Supports exposure-based safety analysis, crash causation modeling, surrogate safety measures, and statistical methods for rare events. Use when analyzing safety data, computing crash rates, or building data-driven safety arguments."
metadata:
  version: "1.0.0"
  last_updated: "2026-03-25"
  author: "AD Safety Research Skills"
  tags: ["autonomous-driving", "crash-data", "safety-metrics", "naturalistic-driving", "data-analysis"]
---

# AD Dataset Analysis

You are an expert in analyzing autonomous driving safety data, with deep knowledge of crash databases, naturalistic driving studies, safety metrics, and statistical methods for transportation safety research.

## Trigger Conditions

Activate when the user:
- Needs to analyze crash or incident data
- Wants to compute safety metrics from driving data
- Asks about exposure-based safety analysis
- Needs to work with FARS, CRSS, GIDAS, or similar databases
- Wants to build data-driven safety arguments for AD
- Needs statistical methods for rare event analysis in transportation

## Data Sources

### US Crash Databases (NHTSA)

**FARS (Fatality Analysis Reporting System)**
- Census of all fatal motor vehicle crashes in the US
- Available: 1975-present (annual release)
- Access: FARS Query System, bulk download, SAS/CSV files
- Key fields: crash type, vehicle type, person type, contributing factors, roadway, weather
- Use: Baseline fatal crash rates, pre/post comparisons, crash type distributions

**CRSS (Crash Report Sampling System)**
- Nationally representative sample of police-reported crashes (all severities)
- Replaced GES (General Estimates System) in 2016
- ~60,000 cases/year, weighted to national estimates
- Use: Crash frequency estimation, severity distribution, trend analysis

**CISS (Crash Investigation Sampling System)**
- Detailed in-depth crash investigation
- Replaced SCI (Special Crash Investigation)
- Vehicle inspection, EDR data, scene documentation
- Use: Detailed crash reconstruction, injury analysis, ADAS effectiveness

**SGO Data (Standing General Order)**
- ADS and L2 ADAS incident reports (mandatory since 2021)
- Reports from manufacturers deploying ADS on public roads
- Use: ADS crash rates, incident patterns, comparison with human drivers

### European Crash Databases

**GIDAS (German In-Depth Accident Study)**
- Detailed on-scene investigation in Hannover and Dresden regions
- ~2000 crashes/year investigated
- Detailed reconstruction, injury coding (AIS), vehicle deformation
- Use: Pre-crash scenario reconstruction, injury mechanism analysis

**CARE (Community Road Accident Database)**
- EU-wide crash database (aggregated national data)
- Use: Cross-country comparison, EU safety trends

**STATS19 (UK)**
- All police-reported road crashes in Great Britain
- Use: UK crash patterns, road safety analysis

### Naturalistic Driving Studies

**SHRP2 NDS (Strategic Highway Research Program)**
- 3,500+ drivers, 50M+ miles, 6 sites across US
- Continuous recording: forward video, driver face, accelerometers, GPS
- 1,500+ crash and near-crash events
- Access: InSight data portal (requires approved research plan)
- Use: Pre-crash driver behavior, exposure analysis, near-miss analysis

**100-Car NDS**
- 100 vehicles, 12-13 months, ~2M miles
- Pioneering NDS with 82 crashes, 761 near-crashes
- Use: Kinematic signatures, surrogate safety measures

**UDRIVE (European NDS)**
- Cars, trucks, scooters across 6 European countries
- Use: European driving behavior, cross-mode analysis

### ADS Fleet Data

**Waymo Safety Reports**
- Published safety reports with crash and disengagement data
- Autonomous miles driven, crash rates per million miles
- Use: ADS safety benchmarking, comparison with human drivers

**Cruise Safety Reports**
- Fleet crash data and safety methodology
- Use: ADS safety analysis (note: check current operational status)

**California DMV Disengagement Reports**
- Annual reports from ADS testing permit holders
- Disengagement rate per mile for each company
- Use: ADS reliability comparison (with caveats — self-reported, non-standardized)

## Safety Metrics Computation

### Crash Rate Metrics

```python
# Crashes per million vehicle miles traveled (VMT)
crash_rate = (num_crashes / total_vmt) * 1_000_000

# Crashes per million vehicle hours traveled (VHT)
crash_rate_time = (num_crashes / total_vht) * 1_000_000

# Fatal crash rate
fatal_rate = (num_fatal_crashes / total_vmt) * 100_000_000  # per 100M VMT

# Injury crash rate by severity (KABCO scale)
# K = Fatal, A = Incapacitating, B = Non-incapacitating, C = Possible, O = No injury
```

### Exposure-Based Analysis

```python
# Relative risk (RR)
RR = (crash_rate_exposed / crash_rate_unexposed)

# Odds ratio (OR) — for case-control studies
OR = (a * d) / (b * c)  # 2x2 contingency table

# Induced exposure method — use not-at-fault drivers as exposure proxy
# Two-vehicle crashes: at-fault vs not-at-fault driver characteristics
exposure_weight = not_at_fault_proportion_by_category
```

### Surrogate Safety Measures (from trajectory data)

```python
# Time-to-Collision (TTC)
TTC = distance / relative_speed  # for approaching vehicles

# Modified TTC (accounts for deceleration)
TTC_modified = (v_lead - v_ego) / (2 * a_max) + distance / v_ego

# Post-Encroachment Time (PET)
PET = t_second_vehicle_at_conflict - t_first_vehicle_clears_conflict

# Deceleration Rate to Avoid Crash (DRAC)
DRAC = (v_ego - v_lead)**2 / (2 * distance)

# Time Exposed TTC (TET)
TET = sum(dt for t in trajectory if TTC(t) < threshold)

# Time Integrated TTC (TIT)
TIT = sum((threshold - TTC(t)) * dt for t in trajectory if TTC(t) < threshold)

# Maximum speed (Vmax), Maximum deceleration
# Lateral acceleration, jerk
```

### Statistical Significance for Safety Claims

```python
# Poisson test for crash rate comparison
from scipy.stats import poisson
# H0: crash_rate_ADS >= crash_rate_human
# Compute p-value using Poisson distribution

# Required sample size for safety demonstration
# (Kalra & Paddock, 2016)
# To demonstrate ADS is X times safer than human:
# Miles needed ≈ (z_alpha + z_beta)^2 / (crash_rate * (1 - 1/X)^2)

# Example: Demonstrate 2x safer than human fatality rate (1.1 per 100M miles)
# Need ~275 million miles (at 95% confidence, 80% power)

# Bayesian approach for small samples
# Prior: informed by human crash rate
# Likelihood: Poisson/negative binomial for crash counts
# Posterior: updated crash rate estimate with credible intervals

# Extreme value theory for near-miss extrapolation
# Fit GPD (Generalized Pareto Distribution) to surrogate safety measure peaks
# Extrapolate to estimate crash probability from near-miss data
```

## Analysis Workflows

### Workflow 1: Crash Type Distribution Analysis
1. Query crash database (FARS/CRSS) for relevant crashes
2. Filter by crash type (e.g., rear-end, angle, pedestrian)
3. Cross-tabulate by contributing factors (distraction, impairment, speed)
4. Compute proportions and confidence intervals
5. Compare with AD-relevant scenarios
6. Identify which crash types AD could prevent and residual crash types

### Workflow 2: ADS Safety Comparison
1. Obtain ADS crash/incident data (SGO reports, company disclosures)
2. Compute ADS crash rate (crashes per million miles, by severity)
3. Obtain comparable human baseline (CRSS weighted, matched conditions)
4. **Critical**: Match on ODD conditions (road type, weather, time of day, geography)
5. Compute relative risk with confidence intervals
6. Sensitivity analysis: vary matching criteria, examine robustness

### Workflow 3: Surrogate Safety from Trajectory Data
1. Extract trajectories from dataset (nuScenes, Waymo, drone data)
2. Identify conflict pairs (vehicles, vehicle-pedestrian, vehicle-cyclist)
3. Compute surrogate safety measures (TTC, PET, DRAC) at each timestep
4. Define thresholds for "critical events" (e.g., TTC < 1.5s)
5. Analyze distribution of critical events
6. Optional: Extreme value modeling to extrapolate crash probability

### Workflow 4: Pre-Crash Scenario Reconstruction
1. Select crash cases from in-depth database (GIDAS, CISS)
2. Reconstruct pre-crash trajectories from EDR/scene data
3. Identify the critical event and contributing factors
4. Map to scenario taxonomy (6-layer model)
5. Parameterize as logical scenarios for simulation replay
6. Test AD system response in reconstructed scenarios

## Common Analysis Tools

```python
# Python ecosystem for AD safety data analysis
import pandas as pd          # Data manipulation
import numpy as np            # Numerical computation
import scipy.stats as stats   # Statistical tests
import statsmodels.api as sm  # Regression, GLM, survival analysis
import matplotlib.pyplot as plt  # Visualization
import seaborn as sns         # Statistical visualization

# Specialized libraries
# pyproj — coordinate transformations
# geopandas — geospatial analysis of crash locations
# lifelines — survival analysis
# pymc / stan — Bayesian analysis
# scikit-learn — clustering crash patterns, classification
```

## Reporting Standards

When presenting safety data analysis:
1. **State the data source, version, and query criteria** explicitly
2. **Report sample sizes** for all subgroups
3. **Use appropriate statistical tests** (don't use t-tests for crash counts — use Poisson/NB regression)
4. **Report confidence intervals** for all rate estimates
5. **Acknowledge data limitations**: underreporting, selection bias, missing variables
6. **Use standard injury scales**: AIS, KABCO, ISS
7. **Specify the comparison baseline** clearly when making safety claims
8. **Avoid ecological fallacy**: aggregate crash rates ≠ individual risk
9. **Account for exposure differences** when comparing populations
10. **Sensitivity analysis**: show how results change with different assumptions

## Key References for Methods

- Hauer, E. (2015). *The Art of Regression Modeling in Road Safety* — foundational text
- Mannering et al. (2016). Unobserved heterogeneity in crash data — methodological review
- Kalra & Paddock (2016). Driving to Safety — miles needed for ADS safety demonstration
- Tarko, A. (2018). *Measuring Road Safety with Surrogate Events* — surrogate safety methods
- Zheng et al. (2019). Extreme value theory for traffic conflict analysis
- Wishart et al. (2020). Safety metrics for ADS — framework and literature review
