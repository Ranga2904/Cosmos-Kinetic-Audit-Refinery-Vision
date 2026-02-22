# 🏗️ KINETIC-AUDIT: Characterizing NVIDIA Cosmos 2B for Industrial Telemetry

### **1. Project Overview**
**KINETIC-AUDIT** evaluates the **NVIDIA Cosmos 2B** foundation model's ability to monitor refinery telemetry. This study uses "video-native" reasoning to detect kinetic drifts in high-hazard environments, identifying the precise boundary between AI reliability and stochastic failure.

---

### **2. Empirical Reliability Map**
This map represents the **True Empirical Performance** of the model as recorded in the project's `statistical_validation.json`. Accuracy is calculated via Mean Absolute Error (MAE) against programmatic Ground Truth.

| Node | Physical Mode | Mean Accuracy | Status | Diagnostic Engineering Insight |
| :--- | :--- | :--- | :--- | :--- |
| **L-102** | Expanding Leak | **64.38%** | ✅ PASS | Validated for high-velocity drift detection. |
| **S-909** | Fatigue Vibration| **64.01%** | ✅ PASS | Successfully resolved low-frequency oscillations. |
| **T-505** | Thermal Runaway | **17.83%** | ❌ FAIL | **Feature Conflation:** Signal lost in dynamic noise. |
| **C-301** | Pressure Surge | **0.00%** | ❌ FAIL | **Total Occlusion:** Temporal HUD failure. |

#### **2.5 Statistical Validation (3-Run Analysis)**
To ensure results are reproducible, each node underwent a 3-run Monte Carlo audit.

| Node | Mean Prediction | Std Dev | Status |
| :--- | :--- | :--- | :--- |
| **L-102** | 279.40 | 1.10 | ✅ Validated |
| **S-909** | 9.85 | 0.04 | ✅ Validated |
| **T-505** | 4.68 | 0.44 | ❌ Feature Conflation |
| **C-301** | 0.00 | 0.00 | ❌ Total Occlusion |



> **Key Finding:** Low standard deviations across runs confirm **deterministic failure modes**. The failures (C-301, T-505) are consistent and reproducible, indicating specific visual-spatial limitations of the video encoder when faced with HUD occlusion.

---

### **3. Engineering Root Cause Analysis (RCA)**

#### **RCA Case A: C-301 Failure (Total Signal Loss)**
* **Evidence:** 0.0% Accuracy across all 3 runs.
* **Failure Analysis:** **Temporal HUD Occlusion.** In this simulation, the rapid pressure surge generated visual artifacts that completely obscured the numerical HUD. Cosmos 2B returned a null value (0.0) as the video encoder could not decouple the telemetry text from the pressure-induced visual noise.

#### **RCA Case B: T-505 Failure (Feature Conflation)**
* **Evidence:** 17.83% Accuracy.
* **Failure Analysis:** **Multi-modal Signal Blending.** During the "Thermal Runaway" sequence, the heat haze effect caused the video encoder to conflate background pixel movement with numerical drift. This resulted in a massive "stutter" in predicted values, dropping accuracy significantly below the industrial safety floor.

---

### **4. Industrial Safety Standards (SIS)**
This project implements a deterministic safety bridge aligned with **IEC 61511** (Functional Safety for Process Industries):

* **Safety Integrity Thresholds:** Nodes L-102 and S-909 meet the "Advisory Monitoring" floor (>60%).
* **Safe-Fail Protocols:** Nodes C-301 and T-505 trigger an immediate **Level 1 Emergency Shutdown (ESD)** due to reliability dropping below the 40% safety floor.



---

### **5. Interactive Demo**
🚀 **Live Dashboard:** [https://huggingface.co/spaces/Ranga2904/kinetic-audit](https://huggingface.co/spaces/Ranga2904/kinetic-audit)

**Local Testing:**
```bash
pip install -r requirements.txt
streamlit run streamlit_app.py
