# Network Traffic Anomaly Detection using Machine Learning

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/BhawookPriyam/Network-Anomaly-Detection-Using-ML/blob/main/Network_Anomaly_Detection_Using_ML.ipynb)

A machine learning project that detects anomalous network traffic using
**Isolation Forest**, an unsupervised anomaly-detection algorithm. It cleans
and prepares raw flow-based network traffic data, engineers a set of
rate/ratio features designed to generalize across different traffic
captures, and classifies each flow as **Normal** or **Anomaly**.

---

## 📌 Project Overview

Network traffic carries a lot of information about how devices communicate.
Spotting traffic that behaves differently from the majority can help flag
potentially suspicious or malicious activity.

The workflow:

1. Load the network traffic dataset
2. Explore the dataset
3. Clean the data (duplicates, infinities, missing values)
4. Remove unsuitable / identifying columns
5. Convert features into numeric form
6. **Engineer rate- and ratio-based features** (see below)
7. Scale the features with `RobustScaler`
8. Train an Isolation Forest model
9. Predict normal and anomalous traffic
10. Evaluate against ground-truth labels
11. Visualize the results
12. Save the trained model and scaler
13. **Score new/unseen traffic from a different source, without retraining**

---

## 🧠 Algorithm & Configuration

**Isolation Forest** isolates unusual observations instead of trying to
learn every possible attack signature — traffic that behaves differently
from the majority gets flagged, regardless of whether it matches a known
attack pattern.

```python
IsolationForest(
    n_estimators=200,
    max_samples=256,
    max_features=0.8,
    contamination="auto",
    random_state=42,
    n_jobs=-1,
)
```

- `n_estimators=200` — more trees → more stable anomaly scores
- `max_samples=256` — the paper-recommended sample size; keeps the model
  from calibrating "normal" around one dataset's particular scale
- `max_features=0.8` — each tree sees a random subset of features, reducing
  overfitting to any single feature or correlation
- `contamination="auto"` — lets the model set its own threshold instead of
  assuming a fixed anomaly rate, which rarely holds across different
  datasets
- `random_state=42` — reproducible results

Features are scaled with **`RobustScaler`** (median/IQR-based) rather than
`StandardScaler`, since network flow data is naturally heavy-tailed and a
handful of extreme flows shouldn't dominate the scale.

---

## 🎯 Engineered Features

Earlier versions of this project used raw magnitudes (byte counts, packet
counts, flow duration). Those scale with how long a capture ran and how
"big" its traffic was, so a value that's normal in one capture can look
extreme in another. The features below describe **how a flow behaves**
rather than **how big it is**, which transfers much better across
different data sources:

| Feature | What it captures |
|---|---|
| `pkts_per_sec`, `bytes_per_sec` | Traffic rate (log-scaled) |
| `avg_pkt_size` | Average packet size |
| `fwd_bwd_pkt_ratio`, `fwd_bwd_byte_ratio` | Flow symmetry (scans/floods are often one-sided) |
| `fwd_pkt_len_mean/std`, `bwd_pkt_len_mean/std` | Packet-size behavior |
| `flow_iat_mean/std`, `fwd_iat_mean`, `bwd_iat_mean` | Inter-arrival timing pattern |
| `syn_ratio`, `rst_ratio`, `fin_ratio`, `psh_ratio`, `ack_ratio` | Protocol-flag behavior, normalized by packet count |
| `init_win_fwd`, `init_win_bwd` | Initial TCP window size |

Column names are resolved through common aliases (e.g. `"Total Fwd Packets"`
vs `"Tot Fwd Pkts"`), and any feature whose source column isn't present in a
given dataset is skipped gracefully rather than breaking the pipeline.

---

## 🔁 Scoring New / Unseen Data

The notebook's last cell defines `score_new_traffic(csv_path)`, which reuses
the **already-fitted** scaler and model on new data — it only calls
`.transform()` / `.predict()`, never `.fit()` again. Re-fitting on every new
file recalibrates "normal" to whatever's in that specific file, which is the
main reason results can look inconsistent across data sources:

```python
new_results = score_new_traffic("Data/other_source_traffic.csv")
new_results.head()
```

---

## 📊 Dataset

This project uses the **CIC-IDS2017** dataset
(https://www.unb.ca/cic/datasets/ids-2017.html). The full dataset is not
included in this repository due to its size — see [`Data/README.md`](Data/README.md)
for setup instructions.

Identifying columns are removed before training:

- `Flow ID`, `Source IP` / `Src IP`, `Destination IP` / `Dst IP`, `Timestamp`

---

## 🛠️ Technologies Used

- Python
- Pandas, NumPy — data manipulation
- Scikit-learn — Isolation Forest, RobustScaler, evaluation metrics
- Matplotlib, Seaborn — visualization
- Joblib — saving/loading the trained model

---

## 📁 Project Structure

```
Network-Anomaly-Detection-Using-ML/
│
├── Network_Anomaly_Detection_Using_ML.ipynb   # Main notebook
├── requirements.txt
├── README.md
├── LICENSE
│
├── Data/
│   ├── README.md              # Dataset setup instructions
│   └── network_traffic.csv    # (not committed — you add this)
│
└── model/
    ├── README.md
    ├── isolation_forest.pkl   # Generated by running the notebook
    └── scaler.pkl             # Generated by running the notebook
```

---

## 🚀 Getting Started

```bash
git clone https://github.com/BhawookPriyam/Network-Anomaly-Detection-Using-ML.git
cd Network-Anomaly-Detection-Using-ML
pip install -r requirements.txt
```

Then add your dataset per [`Data/README.md`](Data/README.md) and run
`Network_Anomaly_Detection_Using_ML.ipynb` top to bottom (locally in
Jupyter, or via the Colab badge above).

---

## 📄 License

Released under the [MIT License](LICENSE).
