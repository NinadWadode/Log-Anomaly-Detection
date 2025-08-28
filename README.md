# Log Anomaly Detection POC — LLM (Gemini) + IsolationForest

A two-stage proof-of-concept to detect anomalies in application logs:

1) **Stage 1 (Cold Start):** Batch logs → prompt **Gemini** to label anomalies and return JSON.  
2) **Stage 2 (Warm Start):** Train **IsolationForest** on LLM-labeled normal logs and run scalable anomaly detection.  
Includes **new-app onboarding** (schema detection/mapping), **compatibility checks**, and **PSI drift** on numeric features.

<p align="center">
  <img src="assets/architecture.png" alt="Architecture" width="760">
</p>

---

## Features

- Synthetic log generator with realistic fields (service, instance, queue, level, status, message, errors).
- Windowing to send **groups of logs** to Gemini (free-tier friendly).
- LLM labeling with per-(service, instance) thresholds or auto-threshold mode.
- JSON parsing with Markdown-fence stripping → **`df_labeled`** (training data).
- LLM accuracy metrics (TP/TN/FP/FN, Accuracy/Precision/Recall/F1).
- **Stage-2 ML:** IsolationForest trained on LLM-labeled **normal** logs; prediction on new data.
- **New app onboarding pipeline**:
  - Detect schema differences (`detect_and_route_new_app`)
  - Normalize/mapping via aliases (`normalize_new_app_schema`)
  - Compatibility & drift check (`check_model_compatibility`) + one-hot feature alignment
- PSI drift labeling: `none / moderate / major`.

---

## Repo Layout

