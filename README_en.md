# EduConectada: Statistical Diagnostics of Brazilian Public School Connectivity

> **Language / Idioma:** **English** | [Portugues](README.md)

![Python](https://img.shields.io/badge/Python-Pandas_%7C_Seaborn-3776AB?logo=python&logoColor=white)
![Statistics](https://img.shields.io/badge/Statistics-Probability_%7C_Inference_%7C_CLT-8A2BE2)
![Dataset](https://img.shields.io/badge/Analyzed_Schools-137,914-success)

> **Executive Summary:** This project evaluates internet connectivity across Brazilian public schools using nationwide data from **EduConectada (137,914 schools)** combined with empirical telemetry from **SIMET (76,359 schools with valid active tests)**. Leveraging exploratory data analysis, sample space stratification (Omega 1 and Omega 2), conditional probability, cumulative distribution functions, and **Central Limit Theorem** simulations, this study investigates: **are there disparities between urban and rural connectivity, and does delivered throughput satisfy estimated per-student demand?**

---

## 1. Context and Sample Space Stratification (Omega 1 vs. Omega 2)

Administrative records across the **137,914 schools** (benchmarkable to the National School Census) provide near-complete coverage for geographic location, administrative dependency, enrollment size, and computer labs, whereas physical network telemetry from SIMET is available for a measured subpopulation.

To enable direct quantitative investigation of actual internet performance, two sample spaces were formalized:

- **Omega 1 (137,914 schools):** Complete administrative universe for physical infrastructure, geographic spread, and declared internet connectivity (`indicador_internet`).
- **Omega 2 (76,359 schools):** Subset with declared internet access and validated TCP telemetry via SIMET (`media_tcp_download`, `media_tcp_upload`, `media_latencia`, `media_jitter_upload`).

> **Methodological note on selection bias:** The definition of Omega 2 was not designed to eliminate selection bias, as schools with active network telemetry may already possess comparatively better infrastructure. The segmentation was created to isolate and examine empirical network metrics where valid measurements exist.

![Selection Bias and Representativeness](./reports/figures/fig_09_vies_selecao_omega2.png)

![School Profiles by Size and Location](./reports/figures/fig_01_perfil_escolas_porte_localizacao.png)

---

## 2. Key Statistical Findings

### A. Positive Skewness and Bandwidth Distributions

Both download and upload throughput display **pronounced positive skewness (heavy right tails)**. A small fraction of schools connected via high-speed fiber links pulls the arithmetic mean upward, establishing the **median** as the most reliable measure for the typical school.

![Download Throughput Distribution](./reports/figures/fig_04_distribuicao_cauda_longa_download.png)

Furthermore, empirical testing reveals strong linear association between download and upload ($r = 0.69$) exhibiting diagonal fan-shaped dispersion, alongside strong correlation between latency and jitter.

![Download vs Upload Dynamics](./reports/figures/fig_03_relacao_download_upload.png)

![Network Metrics Correlation Matrix](./reports/figures/fig_02_matriz_correlacao_metricas.png)

---

### B. The Urban-Rural Paradox (Conditional Probability)

#### 1. Nominal Bandwidth (Mbps)
In nominal terms, **urban** schools achieve a median download throughput roughly **double** that of **rural** schools.

![Urban vs Rural Bandwidth](./reports/figures/fig_05_velocidade_urbana_vs_rural.png)

#### 2. Per-Student Demand Fulfillment
When comparing measured throughput against recommended capacity (benchmarked to allow simultaneous video streaming for 50% of enrolled students), **76.4% of evaluated schools fail to meet minimum estimated demand**.

![Delivered vs Required Bandwidth](./reports/figures/fig_06_velocidade_necessaria_vs_entregue.png)

#### 3. The Fulfillment Paradox
Calculating **conditional probability** for demand satisfaction by geographic location:
- **P(Sufficient | Rural) = 30.17%**
- **P(Sufficient | Urban) = 21.64%**

A rural public school demonstrates a higher probability of satisfying its student bandwidth demand than an urban school.

![Urban vs Rural Demand Fulfillment Paradox](./reports/figures/fig_07_paradoxo_atendimento_urbano_rural.png)

#### Underlying Mechanism
This counterintuitive outcome stems from cumulative enrollment distributions. Urban schools concentrate substantially larger student bodies. Although receiving higher nominal throughput, distributing bandwidth across extensive student cohorts produces recurring deficits, whereas smaller rural schools experience less severe per-student constraints.

![Cumulative Probability by School Size](./reports/figures/fig_08_porte_escolas_probabilidade_acumulada.png)

---

## 3. Central Limit Theorem (Sampling Simulation)

Using the `media_tcp_download` metric, sampling convergence was simulated across expanding sample sizes ($n = 1 \rightarrow n = 30 \rightarrow n = 100 \rightarrow n = 500$). 

The simulation demonstrates that as sample size increases, the distribution of sample means converges to a Gaussian (normal) distribution, attenuating variance inversely proportional to the square root of $n$, as proven by the Central Limit Theorem.

---

## 4. Repository Structure

```text
educonectada-school-connectivity/
├── README.md                            # Methodology and analytical findings (Portuguese)
├── README_en.md                         # Methodology and analytical findings (English)
├── notebooks/
│   └── educonectada.ipynb               # Python notebook with data pipeline and statistical tests
└── reports/
    └── figures/                         # High-resolution figures and presentation visual assets
```
