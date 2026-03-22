# **`README.md`**

# Extreme Value Analysis for Finite, Multivariate and Correlated Systems

<!-- PROJECT SHIELDS -->
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.9%2B-blue.svg)](https://www.python.org/)
[![arXiv](https://img.shields.io/badge/arXiv-2603.05260-b31b1b.svg)](https://arxiv.org/abs/2603.05260)
[![Journal](https://img.shields.io/badge/Journal-ArXiv%20Preprint-003366)](https://arxiv.org/abs/2603.05260)
[![Year](https://img.shields.io/badge/Year-2026-purple)](https://github.com/extreme_value_analysis_correlated_systems)
[![Discipline](https://img.shields.io/badge/Discipline-Econophysics%20%7C%20Quantitative%20Finance-00529B)](https://github.com/extreme_value_analysis_correlated_systems)
[![Data Sources](https://img.shields.io/badge/Data-NYSE%20Daily%20TAQ-lightgrey)](https://www.nyse.com/data/transactions-statistics-data-library)
[![Core Method](https://img.shields.io/badge/Method-PCA--Whitening%20%7C%20POT%20EVT-orange)](https://github.com/extreme_value_analysis_correlated_systems)
[![Analysis](https://img.shields.io/badge/Analysis-Spectral%20Decomposition%20%7C%20Non--Stationary%20Time%20Series-red)](https://github.com/extreme_value_analysis_correlated_systems)
[![Validation](https://img.shields.io/badge/Validation-Kolmogorov--Smirnov%20%7C%20NRMSD-green)](https://github.com/extreme_value_analysis_correlated_systems)
[![Robustness](https://img.shields.io/badge/Robustness-Rolling%20Windows%20%7C%20Threshold%20Sensitivity-yellow)](https://github.com/extreme_value_analysis_correlated_systems)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Type Checking: mypy](https://img.shields.io/badge/type%20checking-mypy-blue)](http://mypy-lang.org/)
[![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=flat&logo=numpy&logoColor=white)](https://numpy.org/)
[![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![SciPy](https://img.shields.io/badge/SciPy-%230C55A5.svg?style=flat&logo=scipy&logoColor=white)](https://scipy.org/)
[![YAML](https://img.shields.io/badge/YAML-%23CB171E.svg?style=flat&logo=yaml&logoColor=white)](https://yaml.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-%23F37626.svg?style=flat&logo=Jupyter&logoColor=white)](https://jupyter.org/)
[![Open Source](https://img.shields.io/badge/Open%20Source-%E2%9D%A4-brightgreen)](https://github.com/extreme_value_analysis_correlated_systems)

**Repository:** `https://github.com/chirindaopensource/extreme_value_analysis_correlated_systems`

**Owner:** 2026 Craig Chirinda (Open Source Projects)

This repository contains an **independent**, professional-grade Python implementation of the research methodology from the 2026 paper entitled **"Extreme Value Analysis for Finite, Multivariate and Correlated Systems with Finance as an Example"** by:

*   **Benjamin Köhler** (Universität Duisburg–Essen)
*   **Anton J. Heckens** (Universität Duisburg–Essen)
*   **Thomas Guhr** (Universität Duisburg–Essen)

The project provides a complete, end-to-end computational framework for replicating the paper's findings. It delivers a modular, out-of-core, and highly optimized pipeline that executes the entire research workflow: from the ingestion and synchronization of asynchronous high-frequency tick data to the spectral decoupling of correlated assets, culminating in the rigorous estimation of non-stationary tail risks using Extreme Value Theory (EVT).

## Table of Contents

- [Introduction](#introduction)
- [Theoretical Background](#theoretical-background)
- [Features](#features)
- [Methodology Implemented](#methodology-implemented)
- [Core Components (Notebook Structure)](#core-components-notebook-structure)
- [Key Callable: `execute_research_pipeline`](#key-callable-execute_research_pipeline)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Input Data Structure](#input-data-structure)
- [Usage](#usage)
- [Output Structure](#output-structure)
- [Project Structure](#project-structure)
- [Customization](#customization)
- [Contributing](#contributing)
- [Recommended Extensions](#recommended-extensions)
- [License](#license)
- [Citation](#citation)
- [Acknowledgments](#acknowledgments)

## Introduction

This project provides a Python implementation of the analytical framework presented in Köhler, Heckens, and Guhr (2026). The core of this repository is the iPython Notebook `extreme_value_analysis_correlated_systems_draft.ipynb`, which contains a comprehensive suite of functions to replicate the paper's findings. 

The pipeline addresses a critical challenge in quantitative finance and complex systems: how to quantify the probability of simultaneous, catastrophic failures in high-dimensional networks where standard multivariate EVT is computationally intractable due to the "Curse of Dimensionality."

The codebase operationalizes the proposed solution:
-   **Synchronizes** asynchronous, millisecond-resolution limit order book updates into a rigid 1-second grid using exact Last-Observation-Carried-Forward (LOCF) imputation.
-   **Decouples** the highly correlated financial network by rotating the system into the eigenbasis of its Pearson correlation matrix (PCA-Whitening).
-   **Estimates** tail risks on the orthogonalized modes using the Peaks-Over-Threshold (POT) approach and Maximum Likelihood Estimation (MLE) of the Generalized Pareto Distribution (GPD).
-   **Neutralizes** non-stationarity by computing intraday volatility profiles and applying rolling-window local thresholds, proving that volatility clustering is largely an artifact of background variance.

## Theoretical Background

The implemented methods combine techniques from Random Matrix Theory (RMT), Microstructure Econometrics, and Extreme Value Theory.

**1. Spectral Decoupling (PCA-Whitening):**
To handle cross-sectional correlations, the normalized return matrix $M$ is rotated into uncorrelated, variance-normalized eigenmodes $R$ using the spectral decomposition of the correlation matrix $C = U \Lambda U^\dagger$:
$$ R = \Lambda^{-1/2} U^\dagger M $$

**2. Peaks-Over-Threshold (POT) EVT:**
The conditional excess distribution over a high threshold $u$ converges to the Generalized Pareto Distribution (GPD). The parameters are estimated via MLE:
$$ G_{\gamma, \sigma}^{\text{GPD}}(x) = \begin{cases} 1 - (1 + \frac{\gamma x}{\sigma})^{-1/\gamma} & \gamma \neq 0 \\ 1 - e^{-x/\sigma} & \gamma = 0 \end{cases} $$

**3. Extremal Clustering:**
The clustering of extreme events is quantified using the Ferro-Segers Extremal Index $\hat{\Theta} \in [0, 1]$, derived from inter-exceedance times $\tau_i$:
$$ \hat{\Theta} = \begin{cases} \min \left( \frac{2 \langle (\tau_i - 1) \rangle^2}{\langle (\tau_i - 1)(\tau_i - 2) \rangle}, 1 \right) & \max \tau_i > 2 \\ \min \left( \frac{2 \langle \tau_i \rangle^2}{\langle \tau_i^2 \rangle}, 1 \right) & \max \tau_i \le 2 \end{cases} $$

**4. Dynamic Residualization:**
To isolate true stochastic risk from deterministic intraday seasonality, the modes are divided by their average volatility profile:
$$ \tilde{R}_k(t) = R_k(t) / V_k(t \pmod{T_{\text{day}}}) $$

Below is a diagram which summarizes the proposed approach:

<div align="center">
  <img src="https://github.com/chirindaopensource/extreme_value_analysis_correlated_systems/blob/main/extreme_value_analysis_correlated_systems_ipo_main.png" alt="Orthogonalized Tail-Risk Engine Architecture" width="100%">
</div>

## Features

The provided iPython Notebook (`extreme_value_analysis_correlated_systems_draft.ipynb`) implements the full research pipeline, including:

-   **Out-of-Core Processing:** Utilizes `numpy.memmap` and chunked BLAS `DGEMM` operations to process over 2.6 billion data points ($479 \text{ stocks} \times 5.4 \times 10^6 \text{ seconds}$) without exhausting standard workstation RAM.
-   **Configuration-Driven Design:** All study parameters (asset universe, session boundaries, rolling window sizes) are managed in an external, cryptographically hashed `config.yaml` file.
-   **Rigorous Data Validation:** A multi-stage validation process checks schema integrity, temporal monotonicity, and numeric stability (e.g., strict positivity of prices, exact trace identities of correlation matrices).
-   **Advanced Optimization:** Employs L-BFGS-B optimization with strict support-constraint penalty barriers and numerically stable limits ($\gamma \to 0$) for GPD fitting.
-   **High-Performance Autocovariance:** Implements an $O(T \log T)$ FFT-based algorithm with zero-padding to compute long-range dependence up to 10,000 lags.

## Methodology Implemented

The core analytical steps directly implement the methodology from the paper:

1.  **Data Engineering (Tasks 1-8):** Ingests raw TAQ data, applies microstructure filters (e.g., $ASK > BID$), synchronizes to a 1-second grid via LOCF, computes log-returns, and standardizes to zero mean and unit variance.
2.  **Spectral Analysis (Tasks 9-12):** Computes the dense Pearson correlation matrix, extracts eigenvalues/eigenvectors, performs PCA-whitening, and validates the economic interpretation of the modes (Market, Energy, Utility).
3.  **Static EVT (Tasks 13-16):** Extracts static POT exceedances, fits the GPD via MLE, computes asymptotic standard errors, and estimates the Ferro-Segers Extremal Index.
4.  **Block Maxima Inference (Tasks 17-18):** Computes absolute-return ACF and analytically maps POT parameters to Generalized Extreme Value (GEV) distributions, validating against empirical daily maxima via KS tests.
5.  **Dynamic Analysis (Tasks 19-20):** Computes intraday volatility profiles, deseasonalizes the modes, and applies rolling-window local thresholds to neutralize non-stationary background variance.
6.  **Robustness & Assembly (Tasks 21-24):** Executes threshold and window-size sensitivity analyses, evaluates the manuscript's qualitative claims using strict quantitative rules, and compiles a cryptographic dossier.

## Core Components (Notebook Structure)

The notebook is structured as a logical pipeline with modular orchestrator functions for each of the 24 major tasks. All functions are self-contained, fully documented with type hints and docstrings, and designed for professional-grade execution.

## Key Callable: `execute_research_pipeline`

The project is designed around a single, top-level user-facing interface function:

-   **`execute_research_pipeline`:** This master orchestrator function runs the entire automated research pipeline from end-to-end. A single call to this function reproduces the entire computational portion of the project, managing out-of-core memory mapping, spectral decomposition, Maximum Likelihood Estimation, and deterministic state reconstruction for sensitivity analysis.

## Prerequisites

-   Python 3.9+
-   Core dependencies: `pandas`, `numpy`, `scipy`, `matplotlib`, `pyyaml`, `psutil`.

## Installation

1.  **Clone the repository:**
    ```sh
    git clone https://github.com/chirindaopensource/extreme_value_analysis_correlated_systems.git
    cd extreme_value_analysis_correlated_systems
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```sh
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install Python dependencies:**
    ```sh
    pip install pandas numpy scipy matplotlib pyyaml psutil
    ```

## Input Data Structure

The pipeline requires a single primary DataFrame (`df_raw_taq`) representing the raw NYSE Daily TAQ Quote file. It must strictly adhere to the following schema:

-   `DATE` (datetime64[ns]): Trading-day identifier.
-   `TIME_M` (timedelta64[ns]): High-frequency timestamp (time since midnight).
-   `SYM_ROOT` (category): Ticker symbol (must match the 479-asset universe).
-   `EX` (category): Exchange code.
-   `BID` (float64): Best bid price.
-   `BIDSIZ` (int64): Bid size.
-   `ASK` (float64): Best ask price.
-   `ASKSIZ` (int64): Ask size.
-   `QU_COND` (category): Quote-condition code.
-   `RAW_ROW_ORDER` (int64): Deterministic tie-break key for millisecond collisions.

*Note: The pipeline includes a synthetic data generator for testing purposes if access to proprietary WRDS/NYSE data is unavailable.*

## Usage

The notebook provides a complete, step-by-step guide. The primary workflow is to execute the final cell, which demonstrates how to load the configuration, generate synthetic data, and use the top-level `execute_research_pipeline` orchestrator:

```python
import os
import yaml
import pandas as pd

# 1. Load the master configuration from the YAML file.
# (Assumes config.yaml is in the working directory)
with open("config.yaml", "r") as f:
    study_config = yaml.safe_load(f)

# 2. Load raw datasets (Example using synthetic generator provided in the notebook)
# In production, load from CSV/Parquet: pd.read_parquet("taq_2014.parquet")
df_raw_taq = generate_synthetic_taq_data(study_config)

# 3. Execute the entire replication study.
master_output = execute_research_pipeline(
    df_raw_taq=df_raw_taq,
    study_configuration=study_config
)

# 4. Access results
if master_output["execution_log"]["pipeline_status"] == "SUCCESS":
    dossier = master_output["baseline_dossier"]
    print("\n[*] Final Scientific Validation Memo:")
    for finding, assessment in dossier["Phase_7_Validation_Memo"].items():
        print(f"    - {finding.replace('_', ' ').title()}: {assessment['status']}")
```

## Output Structure

The pipeline returns a master dictionary containing:
-   **`baseline_dossier`**: The complete, hierarchical state of the baseline run, including:
    -   `Phase_0_Configuration`: Cryptographic hashes and schema reports.
    -   `Phase_1_Data_Engineering`: Cleaning audits and matrix dimensions.
    -   `Phase_2_Spectral_Analysis`: Eigenvalue histograms and sector concentrations.
    -   `Phase_3_Static_EVT`: GPD parameters ($\hat{\gamma}, \hat{\sigma}$) and Extremal Indices ($\hat{\Theta}$).
    -   `Phase_4_ACF_and_Block_Maxima`: FFT-based autocovariance and GEV inference.
    -   `Phase_5_Dynamic_Analysis`: Rolling-window EVT parameters.
    -   `Phase_7_Validation_Memo`: Formal confirmation of the manuscript's claims.
-   **`execution_log`**: Timing, peak memory consumption (RSS), and validation invariant summaries.
-   **`robustness_report`**: Results of the threshold and rolling-window sensitivity analyses.

## Project Structure

```
extreme_value_analysis_correlated_systems/
│
├── extreme_value_analysis_correlated_systems_draft.ipynb   # Main implementation notebook
├── config.yaml                                             # Master configuration file
├── requirements.txt                                        # Python package dependencies
│
├── LICENSE                                                 # MIT Project License File
└── README.md                                               # This file
```

## Customization

The pipeline is highly customizable via the `config.yaml` file. Users can modify study parameters such as:
-   **Asset Universe:** Change the 479 tickers to analyze different indices (e.g., Russell 2000, NASDAQ 100).
-   **Temporal Boundaries:** Adjust `session_start_inclusive` and `delta_t_seconds` to analyze different market regimes or frequencies (e.g., 5-minute bars).
-   **EVT Parameters:** Modify the `tail_quantile_grid_alpha` to probe deeper into the tails.
-   **Dynamic Analysis:** Change the `rolling_window_size_W_seconds` to test different definitions of "local" volatility.

## Contributing

Contributions are welcome. Please fork the repository, create a feature branch, and submit a pull request with a clear description of your changes. Adherence to PEP 8, strict type hinting, and comprehensive docstrings is required.

## Recommended Extensions

Future extensions could include:
-   **Non-Linear Dependencies:** Replacing the Pearson correlation matrix with tail-dependence matrices or copula-based dependency structures.
-   **Real-Time Streaming:** Adapting the out-of-core memmap architecture to ingest live FIX/ITCH feeds for real-time systemic risk monitoring.
-   **Alternative Distributions:** Implementing the Generalized Error Distribution (GED) or extending the framework to multivariate GEV models.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Citation

If you use this code or the methodology in your research, please cite the original paper:

```bibtex
@article{kohler2026extreme,
  title={Extreme Value Analysis for Finite, Multivariate and Correlated Systems with Finance as an Example},
  author={K{\"o}hler, Benjamin and Heckens, Anton J. and Guhr, Thomas},
  journal={arXiv preprint arXiv:2603.05260},
  year={2026}
}
```

For the implementation itself, you may cite this repository:
```
Chirinda, C. (2026). Extreme Value Analysis for Finite, Multivariate and Correlated Systems: An Open Source Implementation.
GitHub repository: https://github.com/chirindaopensource/extreme_value_analysis_correlated_systems
```

## Acknowledgments

-   Credit to **Benjamin Köhler, Anton J. Heckens, and Thomas Guhr** for the foundational research that forms the entire basis for this computational replication.
-   This project is built upon the exceptional tools provided by the open-source community. Sincere thanks to the developers of the scientific Python ecosystem, including **Pandas, NumPy, SciPy, and Psutil**.
