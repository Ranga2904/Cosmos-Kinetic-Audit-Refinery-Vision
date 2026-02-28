# 🏗️ KINETIC-AUDIT: Industrial Reliability Framework for NVIDIA Cosmos 2B

**Quantifying Physical AI Trust for High-Hazard Asset Integrity (Oil & Gas / Chemicals)**

---

## 1. The Industrial Problem
In chemical processing, a "False Negative" in monitoring leads to catastrophic containment loss ($4.7M average cost per incident, CSB 2021). Foundation models lack the deterministic transparency required for safety-critical deployment. 

**KINETIC-AUDIT** transforms NVIDIA Cosmos 2B from a video-describer into a **Virtual Process Sensor**. We provide a statistical audit of **Kinetic Reasoning**—mapping the model's understanding of temporal drift against deterministic ground truth.

## 2. Reliability Map (3-Run Monte Carlo Validation)
This map identifies the **Safe Operating Envelope** for the model based on empirical 3-run mean accuracy. 

| Asset Node | Physical Mode | Mean Accuracy | Status | Process Engineering Diagnosis |
| :--- | :--- | :--- | :--- | :--- |
| **T-505** | Thermal Runaway | **95.28%** | ✅ PASS | **Production-Ready.** Robust temporal gradient resolution. |
| **S-909** | Fatigue Drift | **70.40%** | 🚨 FAIL | **Boundary Identified.** Kinetic Sensitivity Floor (Sub-1%). |
| **C-301** | Pressure Surge | **63.99%** | 🚨 FAIL | **Trend-Aware.** Validated for non-critical advisory only. |
| **L-102** | VOC Leak | **0.00%** | 🚨 FAIL | **Feature Conflation.** Vapor density occluded HUD telemetry. |

## 3. The "Sensitivity Floor" & "Feature Conflation" (Engineering Boundaries)
Unlike "perfect" black-box prototypes, KINETIC-AUDIT identifies the **Probability of Failure on Demand (PFD)** by embracing radical honesty in failure cases.

### Case Study: Node S-909 (Sensitivity Floor)
* **Observation:** Model resolved 70.40% of the drift but failed to hit the 95% safety threshold.
* **Engineering Root Cause:** We identified a **Kinetic Sensitivity Floor**. When visual deltas between adjacent frames are minimal (sub-1% per-frame), the temporal latent space filters the change as background noise.
* **Mitigation:** Requires **Region-of-Interest (RoI) magnification** to resolve micro-drifts.

### Case Study: Node L-102 (Feature Conflation)
* **Observation:** 0% accuracy despite clear visual cues of a leak.
* **Engineering Root Cause:** **Feature Conflation**. The high-entropy visual pixels of the "vapor cloud" interfered with the model's ability to isolate and reason over the numeric HUD.
* **Mitigation:** Industrial safety mandates **redundant non-visual sensors** (e.g., LEL detectors) for this asset class.

## 4. Deterministic Safety Logic (SIS Integration)
Kinetic-Audit bridges AI reasoning with **Safety Instrumented Systems (SIS)**. The integrated Streamlit dashboard enforces a dynamic SIL thresholding logic:

* **Autonomous (>95%):** Model triggers automated PID modulation recommendations.
* **Advisory (85-95%):** Trigger Human-Machine Interface (HMI) for secondary verification.
* **Critical (<85%):** Trigger **Emergency Shutdown (ESD)** recommendation due to model boundary violation.

## 5. Technical Differentiators
* **Temporal Momentum:** Benchmarked against PaliGemma; Cosmos 2B succeeded because it understands **Physical Momentum**—the "Kinetic" in Kinetic-Audit.
* **Edge-Ready (NVIDIA T4):** Optimized via 4-bit quantization and `torchcodec`, ensuring high-throughput inference for refinery-wide scale-out.
* **Audit Trail:** Every inference is logged in `statistical_validation.json`, providing the immutable audit trail required by insurance and safety regulators.

## 6. Deployment (Judge's Guide)

### Prerequisites
* NVIDIA GPU (T4/L4/A10 tested)
* Hugging Face CLI Login (Required for gated Cosmos weights)

### Quick Start
```bash
git clone [https://github.com/YOUR_USERNAME/Kinetic-Audit.git](https://github.com/YOUR_USERNAME/Kinetic-Audit.git)
cd Kinetic-Audit
pip install -r requirements.txt
streamlit run streamlit_app.py
