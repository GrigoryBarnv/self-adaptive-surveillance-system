# Self-Adaptive Surveillance System

Research artifact and implementation repository for a bachelor thesis on adaptive environmental monitoring in a cool-storage setting. The repository combines thesis material, sensor hardware code, simulation environments, analysis notebooks, generated plots, and documentation of the experimental setup.

The main focus is the evaluation of self-adaptive monitoring strategies for temperature and humidity sensors under different energy assumptions and adaptation mechanisms:

- `SamplingRateAdaptation`: adapt how often a sensor samples.
- `StructuralAdaptation`: adapt sensor behavior using structural or rule-based context.
- `mit100Energie`: scenario with a full energy budget.
- `ohne100Energie`: scenario without the 100% energy assumption.

## Table of Contents

- [What This Repository Contains](#what-this-repository-contains)
- [Repository Map](#repository-map)
- [Where To Start](#where-to-start)
- [Simulation Variants](#simulation-variants)
- [Core Workflows](#core-workflows)
- [Quick Start](#quick-start)
- [Technology Stack](#technology-stack)
- [Reproducibility Notes](#reproducibility-notes)
- [Naming Guide](#naming-guide)

## What This Repository Contains

This is not just a code folder. It is a full thesis artifact with several layers:

- final thesis document as PDF
- source code for Arduino-based data logging
- Python simulation environments for adaptive monitoring
- evaluation notebooks for temperature and humidity experiments
- generated figures used for interpretation and reporting
- documentation of the physical setup and experimental procedure

In practice, the repository captures the full path from real-world data collection to simulation-based evaluation.

## Repository Map

```text
self-adaptive-surveillance-system/
+-- 2025_BachelorsThesis_EH_Grigory_Baranov.pdf
+-- BA_Abbildungen/                  # Exported figures used in analysis/reporting
+-- BA_Code_Analyse/                 # Final analysis notebooks and result artifacts
`-- BA_Code_Simulation/              # Main implementation and simulation environments
    +-- AdaptivesMonitoring_mit100Energie/
    `-- AdaptivesMonitoring_ohne100Energie/
```

### Top-Level Directories

| Path | Purpose |
| --- | --- |
| `2025_BachelorsThesis_EH_Grigory_Baranov.pdf` | Final thesis document and best high-level entry point for the research context. |
| `BA_Abbildungen/` | Collected figures for activity analysis, energy curves, distribution plots, mean deviation, and triplet frequency. |
| `BA_Code_Analyse/` | Postprocessed experiment and analysis folders for the final comparison setups. |
| `BA_Code_Simulation/` | Complete simulation codebase for both energy scenarios, including hardware, notebooks, configs, and outputs. |

### Simulation Substructure

Each simulation branch contains the same main building blocks:

| Folder | Role |
| --- | --- |
| `Arduino-Code/` | Firmware for the temperature/humidity data logger. |
| `Documentation/` | Photos, setup sketches, protocol files, and experiment planning material. |
| `SamplingRateAdaptation/` | Simulator, configs, adaptive sensor classes, data processing notebooks, and generated plots for sampling-rate studies. |
| `StructuralAdaptation/` | Rule-based/structural simulation environment and evaluation workflow. |

## Where To Start

Use this section as a navigation shortcut depending on what you want to do.

| Goal | Start Here |
| --- | --- |
| Understand the full project scope | `2025_BachelorsThesis_EH_Grigory_Baranov.pdf` |
| Inspect the main codebase | `BA_Code_Simulation/` |
| Run the sampling-rate simulator without the 100% energy assumption | `BA_Code_Simulation/AdaptivesMonitoring_ohne100Energie/SamplingRateAdaptation/` |
| Run the sampling-rate simulator with the 100% energy scenario | `BA_Code_Simulation/AdaptivesMonitoring_mit100Energie/SamplingRateAdaptation/` |
| Inspect structural adaptation logic | `BA_Code_Simulation/*/StructuralAdaptation/SimulationEnvironment/` |
| Review final analysis notebooks | `BA_Code_Analyse/` |
| Browse publication/report figures | `BA_Abbildungen/` |
| Inspect the sensor logger firmware | `BA_Code_Simulation/*/Arduino-Code/temphum_logger/temphum_logger.ino` |

## Simulation Variants

The repository is organized around two major scenario families and two adaptation families.

### Energy Scenarios

| Scenario | Folder | Intended Use |
| --- | --- | --- |
| With 100% energy | `AdaptivesMonitoring_mit100Energie/` | Baseline or comparison scenario with a full energy assumption. |
| Without 100% energy | `AdaptivesMonitoring_ohne100Energie/` | More constrained scenario for evaluating adaptive behavior under reduced energy assumptions. |

### Adaptation Families

| Family | Main Entry Point | Notes |
| --- | --- | --- |
| Sampling rate adaptation | `SamplingRateAdaptation/SimulationEnvironment/main_simulation.py` | Tests how sampling intervals change over time for temperature and humidity sensors. |
| Structural adaptation | `StructuralAdaptation/SimulationEnvironment/main_simulation.py` | Uses event logs and rule-based logic to evaluate structurally adaptive behavior. |

### Implemented Sampling-Rate Approaches

Across the sampling-rate simulators, the codebase includes retrospective and predictive implementations such as:

- mean-based approaches
- Bollinger-band variants
- confidence interval methods
- fuzzy logic methods
- Kalman-filter methods
- ARIMA-based approaches
- Adam/PEWMA variants
- FAST
- Fisher information / Gaussian-process inspired variants
- matrix profile and change-point based methods
- threshold and tabu-search variants

These are implemented as modular sensor classes under the `SensorClass/` subdirectories and are selected through JSON configuration files.

## Core Workflows

### 1. Data Collection

Physical measurements are collected via Arduino-based temperature/humidity loggers. Setup photos, sketches, and experiment plans are stored under the `Documentation/` folders.

### 2. Preprocessing and Simulation

The simulation environments load preprocessed datasets, instantiate adaptive sensor classes, generate parameter combinations from JSON configuration files, and execute simulation runs in parallel.

Important simulator entry files:

- `SamplingRateAdaptation/SimulationEnvironment/main_simulation.py`
- `StructuralAdaptation/SimulationEnvironment/main_simulation.py`
- `SamplingRateAdaptation/SimulationEnvironment/config_*.json`
- `StructuralAdaptation/SimulationEnvironment/config_structural.json`

### 3. Evaluation and Plotting

After simulation, notebooks in `DataProcessing/` and top-level analysis notebooks in `BA_Code_Analyse/` are used to:

- compare adaptive strategies
- analyze energy behavior
- inspect active/passive states
- evaluate mean deviation and distribution behavior
- generate publication-ready plots

### 4. Reporting

Figures under `BA_Abbildungen/` and the thesis PDF consolidate the results for presentation and interpretation.

## Quick Start

The repository contains multiple runnable simulation environments. A practical starting point is the sampling-rate simulator in the `ohne100Energie` branch.

### Example: Sampling Rate Adaptation

```powershell
cd BA_Code_Simulation\AdaptivesMonitoring_ohne100Energie\SamplingRateAdaptation\SimulationEnvironment
python -m venv venv
.\venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python main_simulation.py
```

### Example: Structural Adaptation

```powershell
cd BA_Code_Simulation\AdaptivesMonitoring_ohne100Energie\StructuralAdaptation\SimulationEnvironment
python -m venv venv
.\venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python main_simulation.py
```

### Configuration Files Worth Checking First

| File | Purpose |
| --- | --- |
| `config_predictive_temp.json` | Predictive simulation setup for temperature. |
| `config_predictive_hum.json` | Predictive simulation setup for humidity. |
| `config_retrospective_temp.json` | Retrospective simulation setup for temperature. |
| `config_retrospective_hum.json` | Retrospective simulation setup for humidity. |
| `config_structural.json` | Structural adaptation setup, including paths and event-log integration. |
| `config_test.json` | Reduced test configuration for smaller runs. |

## Technology Stack

The Python-based simulation environments use:

- `pandas`
- `numpy`
- `scipy`
- `scikit-learn`
- `scikit-fuzzy`
- `stumpy`
- `ruptures`
- `statsmodels`
- `networkx`

In addition, the repository uses:

- Jupyter notebooks for preprocessing, evaluation, and plotting
- Arduino code for embedded data logging
- CSV datasets and exported image/PDF artifacts for result tracking

## Reproducibility Notes

- The repository contains generated outputs in addition to source code. That is intentional: this is an artifact-style research repository.
- Some directories contain large volumes of result files and plots. Readers should treat these as experiment outputs, not hand-maintained source files.
- The branch-specific folders under `BA_Code_Simulation/` are parallel experiment environments rather than duplicates by accident.
- Many final comparison notebooks live in `BA_Code_Analyse/`, while the simulator-specific notebooks live inside each simulation branch.
- The simulator code is driven by configuration files, so most experiment changes start with editing JSON rather than editing Python entry points.

## Naming Guide

Several directory names are in German. This quick mapping makes the repository easier to scan:

| Name | Meaning |
| --- | --- |
| `BA` | Bachelor thesis |
| `Abbildungen` | Figures / illustrations |
| `Code_Analyse` | Analysis code |
| `Code_Simulation` | Simulation code |
| `mit100Energie` | With 100% energy |
| `ohne100Energie` | Without 100% energy |
| `Feuchtigkeit` | Humidity |
| `Temperatur` | Temperature |
| `Versuchsaufbau` | Experimental setup |
| `Versuchsablauf` | Experiment procedure |

## Recommended Reading Order

If you are new to the repository, this sequence is the fastest way to build context:

1. Read the thesis PDF for the research question and evaluation goals.
2. Inspect `BA_Code_Simulation/` to understand the executable simulation environments.
3. Open the relevant `main_simulation.py` and `config_*.json` files for the scenario you care about.
4. Review `BA_Code_Analyse/` for final notebook-based comparisons.
5. Use `BA_Abbildungen/` when you need exported result figures rather than raw notebook output.

## Summary

This repository documents a full adaptive-monitoring research workflow: sensor setup, data collection, simulation, evaluation, and reporting. The most important folders for day-to-day work are `BA_Code_Simulation/` for execution, `BA_Code_Analyse/` for final notebook analysis, and `BA_Abbildungen/` for prepared visual outputs.
