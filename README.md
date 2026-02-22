# 🏗️ KINETIC-AUDIT: Characterizing NVIDIA Cosmos 2B for Industrial Telemetry

### **1. Project Overview**
**KINETIC-AUDIT** is a technical feasibility study evaluating the **NVIDIA Cosmos 2B** foundation model's ability to monitor refinery telemetry. This project moves beyond static OCR by testing "video-native" reasoning to detect kinetic drifts and high-frequency oscillations in real-world industrial scenarios.

---

### **2. Empirical Reliability Map**
In the spirit of **Scientific Rigor**, this report provides the full performance spectrum, including catastrophic failures. Accuracy is calculated via an automated `EmpiricalAuditor` comparing AI predictions against programmatic Ground Truth (GT).

| Node | Physical Mode | GT Drift | AI Prediction | Accuracy | Status | Diagnostic Engineering Insight |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **C-301** | Pressure Surge | 51.88 | 34.54 | **66.58%** | ✅ PASS | Reliable for macro-trend gradient detection. |
| **T-505** | Thermal Runaway | 26.27 | 16.64 | **63.34%** | ✅ PASS | Validated for slow-gradient thermal drifts. |
| **L-102** | Expanding Leak | 433.95 | 1276.21 | **0.00%** | ❌ FAIL | Spatial interference: Leak expansion conflated with digits. |
| **S-909** | Fatigue Vibration| 15.38 | 0.04 | **0.26%** | ❌ FAIL | Nyquist Limit: $f_{sig} (8Hz) > f_{nyq} (5Hz)$. Signal aliased. |

#### **2.5 Statistical Validation (3-Run Analysis)**
To quantify model consistency, each scenario was evaluated across 3 independent inference runs:

| Node | Ground Truth | Mean Prediction | Std Dev | 95% Confidence Interval | Mean Accuracy | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **C-301** | 51.88 | 33.81 ± 0.73 | 0.73 | [32.00, 35.62] | 65.17% | ✅ Validated |
| **T-505** | 26.27 | 16.80 ± 0.50 | 0.50 | [15.57, 18.03] | 63.96% | ✅ Validated |
| **L-102** | 433.95 | 1282.87 ± 44.58 | 44.58 | [1172.12, 1393.62] | 0.00% | ❌ Failed |
| **S-909** | 15.38 | 0.04 ± 0.00 | 0.00 | [0.04, 0.04] | 0.26% | ❌ Failed |

> **Key Finding:** Low standard deviations across runs confirm **deterministic failure modes** rather than random variance. The failures (L-102, S-909) are consistent and reproducible, indicating architectural/physical limitations rather than stochastic behavior.

---

### **3. Root Cause Analysis (RCA)**

#### **RCA Case A: S-909 Failure (Sensitivity Floor)**
* **Evidence:** AI reported 0.04 drift against GT of 15.38 (**0.26% accuracy**).
* **Failure Analysis:** The model failed to track subtle, monotonic declines (100% → 84% over 5s). This represents a **sensitivity limitation** for gradual changes.
* **Engineering Implication:** Cosmos 2B requires either higher frame rate sampling or **Region-of-Interest (RoI) masking** to focus attention on HUD elements for sub-pixel drift detection.



#### **RCA Case B: L-102 Failure (Feature Conflation)**
* **Evidence:** AI predicted 1276.21 vs GT 433.95 (**194% error**).
* **Failure Analysis:** Massive over-prediction suggests **feature conflation**. The model incorporated dynamic background motion (leak expansion pixels) into its numerical interpretation of HUD text.
* **Engineering Insight:** The video encoder blended dynamic HUD overlays with physical scene changes. Mitigation requires separate spatial attention masking to isolate telemetry from environmental hazards.

---

### **4. Methodology & Safety Compliance**
To prevent **Fabrication Bias**, the project aligns with **IEC 61511** (Functional Safety for Process Industries):

1.  **Synthetic Twin Generation:** Programmatic creation of telemetry videos using OpenCV for 100% tamper-proof metadata.
2.  **Safety Thresholding:** A 95% accuracy requirement was set for "Stabilization Protocols." As no node reached this, the system defaulted to **"Safe-Fail" Emergency Shutdown (ESD)** logic.

#### **Risk Reduction Strategy:**
* **Accuracy ≥75%:** Trusted for advisory monitoring.
* **40% ≤ Accuracy <75%:** Flagged for Operator Review (Human-in-the-Loop).
* **Accuracy <40%:** AI Bypassed; system defaults to **Safe-State (ESD Level 1)**.

---

### **5. Limitations & Future Work**
The 0.00% and 0.26% scores represent critical boundaries for foundation model deployment in heavy industry.

* **Hardware-Model Coupling:** Future iterations must sync token sampling rate with physical Nyquist frequency. Node S-909 requires a minimum of **25 FPS** processing.
* **Attention Masking:** Isolation of HUD coordinates during the cross-attention phase is required to mitigate "Feature Conflation" in high-motion environments.

---

### **6. Industrial Impact**
By **honest characterization of failure modes**, this project provides a **Reliability Envelope** for AI safety. We demonstrated that while current models can monitor steady-state drifts (C-301/T-505), they cannot yet serve as primary safety instruments for high-frequency or high-occlusion events without architectural modifications.

---

### **7. Interactive Demo**
🚀 **Live Dashboard:** [https://huggingface.co/spaces/Ranga2904/kinetic-audit](https://huggingface.co/spaces/Ranga2904/kinetic-audit)

**Local Testing:**
```bash
pip install -r requirements.txt
streamlit run streamlit_app.py
