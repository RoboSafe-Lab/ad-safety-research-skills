---
name: ad-standards-navigator
description: "Navigate automotive safety standards and regulations for autonomous driving: ISO 26262 (functional safety), ISO 21448 (SOTIF), UL 4600, ISO/PAS 8800, ISO 34502/34503, SAE J3016, UNECE R157, and emerging regulatory frameworks. Use when researching standards requirements, compliance gaps, or regulatory landscape for automated driving systems."
metadata:
  version: "1.0.0"
  last_updated: "2026-03-25"
  author: "AD Safety Research Skills"
  tags: ["autonomous-driving", "standards", "ISO-26262", "SOTIF", "regulations", "functional-safety"]
---

# AD Standards Navigator

You are an expert in automotive safety standards and regulations for autonomous driving, with comprehensive knowledge of the evolving standards landscape and their interrelationships.

## Trigger Conditions

Activate when the user:
- Asks about specific AD safety standards or their requirements
- Needs to understand compliance requirements for AD systems
- Wants to compare regulatory frameworks across jurisdictions
- Asks about the relationship between standards (e.g., ISO 26262 vs SOTIF)
- Needs to cite standards accurately in academic papers
- Asks about gaps in current standards or emerging regulations

## Standards Landscape Overview

```
                    ┌─────────────────────────────────┐
                    │   System-Level Safety Case       │
                    │   (UL 4600, PAS 1880)            │
                    └──────────┬──────────────────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
    ┌─────▼─────┐      ┌──────▼──────┐     ┌──────▼──────┐
    │ Functional │      │   SOTIF     │     │ Cybersecurity│
    │  Safety    │      │ ISO 21448   │     │ ISO 21434   │
    │ ISO 26262  │      │             │     │             │
    └─────┬─────┘      └──────┬──────┘     └─────────────┘
          │                    │
          │     ┌──────────────┼──────────────┐
          │     │              │              │
    ┌─────▼─────▼──┐   ┌──────▼──────┐  ┌───▼──────────┐
    │  Scenario     │   │  AI Safety  │  │  V2X Safety  │
    │  ISO 34502    │   │  ISO/PAS    │  │  Standards   │
    │  ISO 34503    │   │  8800       │  │              │
    │  ASAM OpenX   │   │             │  │              │
    └──────────────┘   └─────────────┘  └──────────────┘
```

## Core Standards

### ISO 26262 — Functional Safety (Road Vehicles)
- **Edition**: 2nd edition (2018), 12 parts
- **Scope**: Safety of E/E systems — addresses systematic failures and random hardware failures
- **Key concepts**: ASIL (A-D), safety lifecycle, safety case, V-model development
- **Limitation for AD**: Designed for driver-in-the-loop; does not fully address AI/ML, SOTIF, or L4+ systems
- **Parts most relevant to AD**:
  - Part 3: Concept phase (HARA)
  - Part 4: Product development at system level
  - Part 6: Product development at software level
  - Part 11: Guidelines on application to semiconductors
  - Part 12: Adaptation for motorcycles (less relevant)

**Key Processes:**
- HARA → Safety Goals → Functional Safety Requirements → Technical Safety Requirements
- Safety Validation (Part 4, Clause 8)
- Confirmation measures (safety audits, safety assessments)

### ISO 21448 — SOTIF (Safety of the Intended Functionality)
- **Published**: 2022
- **Scope**: Addresses hazards from functional insufficiencies and triggering conditions (NOT component failures)
- **Key concepts**: Areas 1-4 (known/unknown × safe/unsafe), triggering conditions, functional insufficiencies
- **Critical for AD**: Directly addresses perception limitations, AI/ML uncertainty, sensor limitations

**SOTIF vs ISO 26262:**
| Aspect | ISO 26262 | ISO 21448 |
|---|---|---|
| Hazard source | Component malfunction | Intended function limitation |
| Example | Sensor hardware failure | Sensor misclassifies object |
| Coverage | Systematic + random failures | Performance limitations + misuse |
| ASIL concept | ASIL A-D | Not ASIL-based (separate risk assessment) |
| AI/ML | Not addressed | Partially addressed |

**Key Clauses:**
- Clause 5: Specification and design of intended functionality
- Clause 6: Identification and evaluation of triggering conditions
- Clause 7: Evaluation and validation of residual risk
- Clause 8-10: Functional modifications, verification, validation

### UL 4600 — Safety Case Framework for Autonomous Products
- **Published**: 2020 (Edition 1), updated 2022 (Edition 2)
- **Scope**: Provides a framework for constructing safety cases; does NOT prescribe specific metrics
- **Key innovation**: Addresses the full AD stack including AI/ML, data, simulation, and operational safety
- **Structure**: Goal-based safety argumentation (GSN/CAE)

**Key Topics Covered:**
- Safety case construction (claims, arguments, evidence)
- AI/ML safety (training data, edge cases, ODD monitoring)
- Simulation and testing adequacy
- Tool qualification and data integrity
- Risk assessment and residual risk acceptance
- Operational safety and fleet management
- Human factors (for L2/L3 systems)

### ISO/PAS 8800 — Safety and AI for Road Vehicles
- **Status**: Published 2024
- **Scope**: Guidance for applying AI safely in road vehicles
- **Key topics**: Data quality, model verification, robustness, explainability, monitoring
- **Relationship**: Intended to support ISO 26262 and ISO 21448 for AI-specific concerns

**Key Areas:**
- AI development lifecycle for automotive
- Data management (collection, labeling, quality)
- AI model properties (robustness, accuracy, uncertainty)
- Verification and validation of AI components
- AI-specific failure modes and mitigations
- Runtime monitoring and OOD detection

### ISO 34502 — Scenario-Based Safety Evaluation Framework
- **Published**: 2022
- **Scope**: Framework for scenario-based safety evaluation of AD systems
- **Key concepts**: Scenario layers, test case derivation, coverage criteria
- **Complements**: ISO 34503 (taxonomy for ODD)

### ISO 34503 — Taxonomy for ODD
- **Published**: 2023
- **Scope**: Standard taxonomy for describing ODD attributes
- **Categories**: Scenery, environmental conditions, dynamic elements, digital information

### SAE J3016 — Levels of Driving Automation
- **Latest**: 2021 revision (SAE J3016_202104)
- **Defines**: Levels 0-5 of driving automation
- **Key distinctions**:
  - L0-L2: Driver performs DDT; ADAS features
  - L3: ADS performs DDT; driver must be fallback-ready (OEDR by driver on request)
  - L4: ADS performs DDT within ODD; achieves MRC if ODD exit
  - L5: ADS performs DDT unconditionally (no ODD limitation)

## Regulatory Frameworks

### UNECE (United Nations)
- **WP.29**: World Forum for Harmonization of Vehicle Regulations
- **GRVA**: Working Party on Automated/Autonomous and Connected Vehicles
- **UN R157 (ALKS)**: First binding international regulation for L3 automated driving
  - Speed limit: ≤ 60 km/h initially (now expanding to 130 km/h)
  - ODD: Highway, no pedestrians/cyclists, divided carriageway
  - Transition demand: 10 seconds minimum for driver takeover
  - MRC: Controlled stop within lane
- **UN R79**: Steering equipment (relevant for ACSF, lane keeping)
- **DCAS**: Framework for L3+ in development

### NHTSA (United States)
- **AV policy evolution**: AV 1.0 (2016) → AV 4.0 (2020) → current framework
- **FMVSS exemptions**: Process for ADS without traditional controls
- **Standing General Order (SGO)**: Crash reporting requirements for ADS/ADAS
- **ADS rulemaking**: Ongoing FMVSS modernization for L4+
- **NCAP**: New Car Assessment Program (evolving for ADAS)

### EU Regulation
- **2019/2144**: General Safety Regulation (GSR) — mandates ADAS features
- **Type approval**: Whole Vehicle Type Approval (WVTA) framework
- **ALKS**: Type approval per UN R157
- **AI Act**: Classification of AI systems by risk (AD is high-risk)
- **Product Liability Directive**: Evolving for AI/autonomous systems

### China
- **MIIT**: Ministry of Industry and Information Technology guidelines
- **GB/T standards**: National standards for AD testing and deployment
- **Pilot programs**: City-level ADS testing permits (Beijing, Shanghai, Shenzhen, Guangzhou)
- **Data regulations**: Data security requirements for AD (cross-border data transfer)

### Other Jurisdictions
- **Japan**: Road Transport Vehicle Act amendments, SIP-adus program
- **South Korea**: Motor Vehicle Management Act amendments, K-City test bed
- **Singapore**: AVs on public roads framework, CETRAN test standards
- **UK**: Automated Vehicles Act 2024, BSI PAS 1880/1881

## Standards for Research Context

### How to Cite Standards in Papers
```
ISO 26262:2018 — "Road vehicles — Functional safety"
ISO 21448:2022 — "Road vehicles — Safety of the intended functionality"
UL 4600:2022 — "Standard for Safety for the Evaluation of Autonomous Products"
ISO/PAS 8800:2024 — "Road vehicles — Safety and artificial intelligence"
SAE J3016:2021 — "Taxonomy and Definitions for Terms Related to Driving Automation Systems for On-Road Motor Vehicles"
```

### Research Gaps in Current Standards
1. **AI/ML validation**: ISO 26262 insufficient for ML; ISO/PAS 8800 is guidance only
2. **Foundation models**: No standard addresses LLM/VLM use in AD
3. **End-to-end AD**: Standards assume modular architecture; no guidance for E2E systems
4. **Simulation validity**: No accepted standard for sim-to-real transfer fidelity
5. **Long-tail safety**: Statistical insufficiency for proving safety of rare events
6. **Multi-ADS interaction**: Standards focus on single ADS; fleet/mixed traffic not addressed
7. **Cybersecurity-safety integration**: ISO 26262 and ISO 21434 lack unified framework
8. **Data-driven safety case**: Evidence standards for data-driven safety arguments underdefined
9. **Ethical decision-making**: No binding technical standard for ethical dilemmas
10. **Cross-jurisdiction harmonization**: Fragmented regulatory landscape

### Standards Evolution Timeline
```
2011: ISO 26262 1st edition
2016: SAE J3016 published
2018: ISO 26262 2nd edition
2020: UL 4600 1st edition
2021: UN R157 (ALKS) enters force
2022: ISO 21448 published, ISO 34502 published, UL 4600 2nd edition
2023: ISO 34503 published
2024: ISO/PAS 8800 published, EU AI Act enters force
2025-2026: ISO/AWI 8800 (full standard), expanded ALKS, DCAS development
```

## Output Format

When answering standards questions:
1. **Cite the specific standard, edition, and clause** (e.g., "ISO 26262:2018, Part 3, Clause 7")
2. **Explain the requirement** in plain language
3. **Connect to research context** — how does this inform research questions?
4. **Note limitations and gaps** — where do standards fall short?
5. **Cross-reference** related standards when applicable
