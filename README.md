# 🏗️ KINETIC-AUDIT: Characterizing NVIDIA Cosmos 2B for Industrial Telemetry

### **1. Project Overview**
**KINETIC-AUDIT** evaluates the **NVIDIA Cosmos 2B** foundation model's ability to monitor refinery telemetry. This study uses "video-native" reasoning to detect kinetic drifts in high-hazard environments, identifying the precise boundary between AI reliability and stochastic failure.

---

### **2. Empirical Reliability Map**
This map represents the **True Empirical Performance** of the model as recorded in the project's `statistical_validation.json`. Accuracy is calculated via Mean Absolute Error (MAE) against programmatic Ground Truth.

| Node | Physical Mode | Mean Accuracy | Status | Diagnostic Engineering Insight |
| :--- | :--- | :--- | :--- | :--- |
| **T-505** | Thermal Runaway | **98.87%** | ✅ PASS | Exceptional resolution of thermal gradients. |
| **C-301** | Pressure Surge | **70.37%** | ✅ PASS | Validated for macro-trend gradient detection. |
| **L-102** | Expanding Leak | **66.74%** | ✅ PASS | Validated for high-velocity drift detection. |
| **S-909** | Fatigue Vibration| **0.14%** | ❌ FAIL | **Sensitivity Limit:** Nyquist Aliasing (Sub-pixel blur). |

#### **2.5 Statistical Validation (3-Run Analysis)**
To ensure results are reproducible, each node underwent a 3-run Monte Carlo audit.

| Node | Mean Prediction | Std Dev | Status |
| :--- | :--- | :--- | :--- |
| **T-505** | 24.92 | 0.34 | ✅ Validated |
| **C-301** | 34.85 | 0.96 | ✅ Validated |
| **L-102** | 277.68 | 3.49 | ✅ Validated |
| **S-909** | 0.02 | 0.00 | ❌ Sensitivity Limit |

> **Key Finding:** Low standard deviations across runs confirm **deterministic failure modes**. The failure in S-909 is consistent and reproducible, indicating specific temporal-resolution limitations of the video encoder when faced with high-frequency oscillations.

---

### **3. Engineering Root Cause Analysis (RCA)**

#### **RCA Case A: S-909 Failure (Sensitivity Floor)**
* **Evidence:** 0.14% Accuracy across all 3 runs.
* **Failure Analysis:** **Nyquist Sampling Limit.** The fatigue vibration frequency (8Hz) exceeded the sampling floor (5Hz) of the 10fps processing rate. Cosmos 2B reported near-zero drift (0.02 units) as the numerical HUD "blurred" into a static average, rendering high-frequency oscillations invisible to the temporal encoder.

#### **RCA Case B: Multi-Node Validation Success**
* **Evidence:** T-505 (98.87%), C-301 (70.37%), and L-102 (66.74%).
* **Engineering Insight:** The model successfully decoupled numerical HUD telemetry from background motion in thermal runaway, pressure surge, and expanding leak scenarios. This validates Cosmos 2B for monitoring slow-to-medium gradient industrial hazards.

---

### **4. Industrial Safety Standards (SIS)**
This project implements a deterministic safety bridge aligned with **IEC 61511** (Functional Safety for Process Industries):

* **Safety Integrity Thresholds:** Nodes T-505, C-301, and L-102 meet the "Advisory Monitoring" floor (>60%).
* **Safe-Fail Protocols:** Node S-909 triggers an immediate **Level 1 Emergency Shutdown (ESD)** due to reliability dropping below the 40% safety floor.

---

### **5. Interactive Demo**
🚀 **Live Dashboard:** [https://huggingface.co/spaces/Ranga2904/kinetic-audit](https://huggingface.co/spaces/Ranga2904/kinetic-audit)

**Local Testing:**
```bash
pip install -r requirements.txt
# Ensure statistical_validation.json is in the data/ directory
streamlit run streamlit_app.py
