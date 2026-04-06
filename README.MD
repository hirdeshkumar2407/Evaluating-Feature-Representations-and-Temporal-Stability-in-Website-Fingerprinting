This README provides an overview of the research project conducted in the **Homework_2.ipynb** notebook, which evaluates methodologies for identifying encrypted website traffic through side-channel analysis.

# Evaluating Feature Representations and Temporal Stability in Website Fingerprinting

## Overview
This project investigates **Website Fingerprinting (WF)**, a side-channel attack where a passive adversary identifies encrypted web activity by analyzing packet metadata such as size and directionality. The research specifically addresses the challenge of **Temporal Drift**, where updates to dynamic website content degrade the accuracy of classification models over time.

## Threat Model
The study assumes a passive adversary capable of intercepting encrypted HTTPS metadata to identify which of ten prominent news platforms a user is visiting. The evaluation focuses on:
* **Feature Evaluation**: Comparing global statistical metrics (Biflow) against sequential structural traces (CUMUL).
* **Temporal Stability**: Measuring model performance decay by testing a "Day 1" trained model against "Day 2" data.

## Methodology
The experimental design consists of two independent tracks utilizing a **$k$-Nearest Neighbors ($k$-NN)** classifier:

### 1. Biflow Tracking (Statistical)
* **Feature Vector**: A 16-dimensional vector capturing the Mean, Maximum, Minimum, and Standard Deviation of both **Packet Length** and **Inter-Arrival Time (IAT)**.
* **Directionality**: Metrics are bifurcated into Outgoing (Uplink) and Incoming (Downlink) traffic.

### 2. Panchenko CUMUL Track (Structural)
* **Cumulative Trace**: Focuses on the "shape" of the data flow by calculating a running sum of signed packet sizes.
* **Dimensionality Reduction**: Utilizes **piecewise linear interpolation** (via SciPy) to reduce each capture to a 20-point trace.

### Data Collection
The dataset was generated using an automated pipeline that visited 10 target news websites 10 times each (100 total captures per day):
* **Tools**: `tcpdump` for raw capture, `curl` for triggering requests, and `tshark` for metadata extraction.
* **Target Sites**: Includes major outlets such as *The Washington Post*, *CNBC*, *NY Post*, and *ABC News*.

## Key Results
The study demonstrates that structural loading signatures are significantly more robust than volume-based metrics:

| Metric | Biflow (Statistical) | CUMUL (Structural) |
| :--- | :--- | :--- |
| **Initial Accuracy (Day 1)** | 86.7% | **93.3%** |
| **Temporal Stability (Day 2)** | 50.0% | **76.7%** |

## Conclusion
The results show that while Biflow models are sensitive to daily content fluctuations, the **CUMUL methodology** maintains superior stability. This confirms that the structural "skeleton" of a website—the sequence and timing of data bursts—is a more reliable identifier for encrypted traffic than aggregate statistics.

## Environment & Dependencies
* **Language**: Python 3
* **Core Libraries**: `pandas`, `numpy`, `scipy`, `scikit-learn`
* **Visualization**: `matplotlib`, `seaborn`
* **Network Tools**: `tcpdump`, `tshark`, `scapy`, `pyshark`