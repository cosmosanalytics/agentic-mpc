# Agentic Model Predictive Control (Agentic MPC)
### Operating in Intelligence Space via Cognitive Supervisory Layers and Parallel Path Integral Rollouts — A Simulation Case Study

[![Paper](https://img.shields.io/badge/Paper-PDF-red.svg)](agentic_mpc_paper.pdf)
[![Live Demo](https://img.shields.io/badge/Live_Demo-GitHub_Pages-blue.svg)](https://cosmosanalytics.github.io/agentic-mpc/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Author:** Zhaoyang Wan, PhD, MBA  
**Repository:** [cosmosanalytics/agentic-mpc](https://github.com/cosmosanalytics/agentic-mpc)  
**Date:** October 2026

---

## 📌 Overview

**Agentic MPC** introduces an architectural framework that bridges high-throughput numerical process control with cognitive supervisory reasoning. Rather than replacing deterministic solvers with unconstrained foundation models, Agentic MPC partitions the control stack into:

1. **The Knower (Neural Reasoner):** Operates in *Intelligence Space*, translating qualitative operator directives, laboratory feed analysis reports, and shift handover advisories into quantitative cost manifold parameter candidates $\theta_{\mathcal{M}} = \{q_{\text{temp}}, q_{\text{barrier}}, u_{\text{target}}, \lambda\}$.
2. **The Doer (Executive Function Harness):** A deterministic verification kernel enforcing Ring 0 operating invariants (actuator saturation bounds, slew limits), executing automated contract tests (< 30 ms), and deploying preemptive circuit breakers below physical safety-instrumented shutdown limits.
3. **Massively Parallel Rollouts:** Real-time Model Predictive Path Integral (MPPI) control executing 1,024 stochastic rollouts via WebGPU / GPU compute shaders.

---

## 🚀 Live Interactive App

Experience the full client-side simulator directly in your browser (zero installation, 100% WebGPU / WebLLM native):

👉 **[Launch Agentic MPC Studio](https://cosmosanalytics.github.io/agentic-mpc/)**

Features:
- **Interactive CSTR Thermal Simulator:** Dynamic Arrhenius reaction kinetics with real-time temperature, reactant concentration, and cooling control plots.
- **Cognitive Advisory Terminal:** Real-time LLM supervisor parsing advance warnings and tuning cost manifolds on the fly.
- **Safety Envelope Monitoring:** Live SIS trip tracking at 385 K with hardware SCRAM trigger detection.

---

## 📑 Research Paper

The formal simulation case study is available in multiple formats:

- 📄 **[PDF Version (7 Pages, Publication-Ready)](agentic_mpc_paper.pdf)**
- 🌐 **[Interactive HTML Version](https://cosmosanalytics.github.io/agentic-mpc/agentic_mpc_paper.html)**
- 📝 **[Word (.docx) Version](agentic_mpc_paper.docx)**
- 📖 **[Markdown Source](agentic_mpc_paper.md)**

---

## 📊 Benchmark Results

Evaluated across **20 randomized severe kinetic surge scenarios** ($C_{A0} \in [+10\%, +30\%]$, $T_0 \in [+5\text{ K}, +12\text{ K}]$, $UA \in [-10\%, -35\%]$) on an exothermic Continuous Stirred-Tank Reactor (CSTR):

| Configuration | Controller Type | Non-Trip RMSE (K) | Overall RMSE (K) | Peak T (K) | Slew (K/s) | SIS Trips | Settled |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Baseline 1a** | Linear MPC ($q_T = 1.0$, No Forecast) | $2.56 \pm 1.54$ | $16.26 \pm 5.52$ | $389.3 \pm 14.2$ | $3.29 \pm 0.95$ | **12 / 20 (60%)** | 8 / 20 (9.8 s) |
| **Baseline 1b** | Linear MPC ($q_T = 10.0$, No Forecast) | $2.26 \pm 1.14$ | $9.49 \pm 5.39$ | $372.8 \pm 13.8$ | $5.50 \pm 1.35$ | **6 / 20 (30%)** | 13 / 20 (8.5 s) |
| **Baseline 2a** | NMPC (L-BFGS-B, No Forecast) | $2.48 \pm 1.52$ | $9.62 \pm 5.37$ | $372.9 \pm 13.7$ | $5.04 \pm 1.29$ | **6 / 20 (30%)** | 13 / 20 (6.9 s) |
| **Ablation 1** | MPPI (Static $\theta^*$, No Forecast) | $3.00 \pm 1.80$ | $12.13 \pm 5.57$ | $378.9 \pm 14.6$ | $2.52 \pm 0.47$ | **8 / 20 (40%)** | 10 / 20 (6.1 s) |
| **Ablation 2** | MPPI + Lagged-UA Filter (No Forecast) | $2.61 \pm 1.34$ | $9.82 \pm 5.39$ | $373.4 \pm 13.9$ | $2.58 \pm 0.35$ | **6 / 20 (30%)** | 13 / 20 (10.8 s) |
| **Baseline 2b** | NMPC (With Forecast Preview) | **$1.47 \pm 0.85$** | **$2.82 \pm 2.93$** | **$356.8 \pm 7.6$** | $6.42 \pm 1.03$ | **1 / 20 (5%)** | **19 / 20 (2.7 s)** |
| **Ablation 3a** | MPPI + Filter (With Forecast Preview) | **$1.75 \pm 1.00$** | **$3.10 \pm 2.98$** | **$357.3 \pm 7.7$** | **$2.80 \pm 0.24$** | **1 / 20 (5%)** | **18 / 20 (5.6 s)** |
| **Ablation 3b** | Rule Supervisor + MPPI | $1.45 \pm 0.68$ | $9.72 \pm 4.92$ | $382.6 \pm 17.6$ | $3.82 \pm 0.33$ | **8 / 20 (40%)** | 12 / 20 (12.4 s) |
| **Full System** | **Anticipatory Schedule (Full Architecture)** | **$0.94 \pm 0.41$** | **$2.15 \pm 2.55$** | **$355.4 \pm 6.1$** | **$3.39 \pm 0.27$** | **1 / 20 (5%)** | **19 / 20 (2.4 s)** |

### Key Benchmark Discoveries:
- **Empirical Feedback Floor (6/20 Trips):** Without advance lookahead, Linear MPC, NMPC, and MPPI all trip on the exact same 6 corner scenarios (Scenarios 4, 5, 8, 16, 18, 20), proving that reactive feedback alone cannot beat Arrhenius thermal runaway under actuator rate limits.
- **The Quenching Pitfall:** Naive rule-based cooling (flat cooling to 280 K) induces severe reactor quenching ($T_{\min} \approx 339\text{--}344\text{ K}$), allowing unreacted feed to accumulate to $C_A \approx 0.58\text{ mol/L}$ and triggering delayed thermal blowout post-surge (8/20 trips).
- **Anticipatory Pre-Cooling Ramp:** Modulating cooling to 282 K maintains thermal absorption margin while avoiding reaction quenching, achieving **$0.94\text{ K}$ Non-Trip RMSE** and **1/20 trips**.

---

## 🔬 Reproducing the Benchmarks

All benchmark results and sweeps are 100% reproducible with standard Python and NumPy:

```bash
# Clone the repository
git clone https://github.com/cosmosanalytics/agentic-mpc.git
cd agentic-mpc

# Install requirements
pip install numpy scipy

# Run the full 20-scenario multi-tier closed-loop benchmark
python benchmark_cstr_unified.py

# Run the 200 s open-loop hold-280 K oracle lead sweep
python benchmark_cstr_unified.py --oracle
```

---

## 📁 Repository Structure

```
├── index.html                   # Live WebGPU/WebLLM interactive simulation app
├── ai_predictive_control_mpc.html# Standalone app file
├── agentic_mpc_paper.pdf        # Complete 7-page research paper PDF
├── agentic_mpc_paper.html       # Interactive paper HTML (KaTeX + theme switcher)
├── agentic_mpc_paper.docx       # Word document version
├── agentic_mpc_paper.md         # Full paper source in Markdown
├── benchmark_cstr_unified.py    # NumPy CSTR benchmark & oracle verification engine
├── cstr_benchmark_summary.md    # Detailed benchmark documentation & analysis
├── cstr_oracle_lead_sweep.csv   # Open-loop hold-280 K sweep data
├── cstr_scenario_leads.csv      # Per-scenario open-loop advance lead data
├── build_paper_pdf.py           # Chrome headless PDF generator (custom margins)
├── generate_paper_html.py       # Paper HTML compiler with print CSS
└── README.md                    # Project overview & reproduction guide
```

---

## 📜 Citation

```bibtex
@article{wan2026agenticmpc,
  title={Agentic Model Predictive Control: Operating in Intelligence Space via Cognitive Supervisory Layers and Parallel Path Integral Rollouts --- A Simulation Case Study},
  author={Wan, Zhaoyang},
  year={2026},
  url={https://github.com/cosmosanalytics/agentic-mpc}
}
```
