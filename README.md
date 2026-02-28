# 🏗️ KINETIC-AUDIT: Industrial Reliability Framework for NVIDIA Cosmos 2B
**Quantifying Physical AI Trust for High-Hazard Asset Integrity (Oil & Gas / Chemicals)**

[![NVIDIA Cosmos 2B](https://img.shields.io/badge/Model-Cosmos%202B-green)](https://huggingface.co/nvidia/Cosmos-1.0-Prompt-Rewrite-2B)
[![NVIDIA T4 Optimized](https://img.shields.io/badge/Compute-NVIDIA%20T4-76b900)](https://www.nvidia.com/en-us/data-center/tesla-t4/)

## 1. The Industrial Problem
In chemical processing, a "False Negative" in monitoring leads to catastrophic containment loss ($4.7M average cost per incident, CSB 2021). Foundation models lack the transparency required for safety-critical deployment. 

**KINETIC-AUDIT** transforms NVIDIA Cosmos 2B from a video-describer into a **Virtual Process Sensor**. We provide the first statistical audit of **Kinetic Reasoning**—mapping the model's understanding of temporal drift against the laws of thermodynamics.

## 2. Reliability Map (3-Run Monte Carlo Validation)
This map identifies the **Safe Operating Envelope** for the model based on empirical 3-run mean accuracy.

| Asset Node | Physical Mode | Mean Accuracy | Status | Process Engineering Diagnosis |
| :--- | :--- | :--- | :--- | :--- |
| **T-505** | Thermal Runaway | **98.87%** | ✅ PASS | **Production-Ready.** Exceptional temporal gradient resolution. |
| **C-301** | Pressure Surge | **70.37%** | ⚠️ ADVISORY | **Trend-Aware.** Validated for operator-in-the-loop alerts. |
| **L-102** | VOC Leak | **66.74%** | ⚠️ ADVISORY | **SNR Decay.** Vapor density occluded numerical HUD telemetry. |
| **S-909** | Fatigue Drift | **0.14%** | 🚨 FAIL | **Boundary Identified.** Kinetic Sensitivity Floor (Sub-1%). |



## 3. The "Sensitivity Floor" (IEC 61511 Engineering Boundary)
Unlike black-box prototypes, KINETIC-AUDIT identifies the **Probability of Failure on Demand (PFD)**.

**Discovery at Node S-909:**
* **Scenario:** Monotonic numerical decline (~0.3% delta per frame).
* **Engineering Root Cause:** We identified a **Kinetic Sensitivity Floor**. When visual deltas between adjacent frames are minimal, the temporal latent space may fail to resolve state changes.
* **Industrial Mitigation:** IEC 61511 compliance for this node requires **Region-of-Interest (RoI) magnification** or **High-Bitrate sampling** to resolve sub-1% drifts.

## 4. Deterministic Safety Logic (SIS Integration)
Kinetic-Audit bridges AI reasoning with **Safety Instrumented Systems (SIS)**. The integrated Streamlit dashboard allows users to set a dynamic SIL threshold:
* **Autonomous (>95%):** Model triggers automated PID modulation recommendation.
* **Advisory (60-90%):** Trigger Human-Machine Interface (HMI) for verification.
* **Failsafe (<40%):** Trigger Emergency Shutdown (ESD) recommendation due to model boundary.

## 5. Technical Differentiators
1.  **Temporal Momentum:** Benchmarked against PaliGemma; Cosmos 2B succeeded because it understands **Physical Momentum**—the "Kinetic" in Kinetic-Audit.
2.  **Edge-Ready (T4 GPU):** Optimized via 4-bit quantization and `torchcodec` for the **NVIDIA T4**, ensuring economic density for refinery-wide scale-out.
3.  **Audit Trail:** Every inference is logged in `statistical_validation.json`, providing the immutable audit trail required by insurance and safety regulators.

## 6. Deployment (Judge's Guide)
### Prerequisites
* NVIDIA GPU (T4/L4/A10 tested)
* Hugging Face CLI Login: `huggingface-cli login` (Required for gated Cosmos weights)

### Quick Start
```bash
git clone [https://github.com/YOUR_USERNAME/Kinetic-Audit.git](https://github.com/YOUR_USERNAME/Kinetic-Audit.git)
cd Kinetic-Audit
pip install -r requirements.txt
streamlit run streamlit_app.py
