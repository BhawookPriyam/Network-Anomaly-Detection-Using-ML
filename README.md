Network Traffic Anomaly Detection using Machine Learning

A machine learning project that detects anomalous network traffic using the Isolation Forest algorithm.

The project processes network traffic data, cleans and prepares the dataset, selects important network-flow features, scales the data, and uses an unsupervised machine learning model to classify network traffic as Normal or Anomaly.

---

📌 Project Overview

Network traffic contains a large amount of information about communication between devices. Identifying unusual traffic can help in detecting potentially suspicious or malicious network activity.

This project uses Isolation Forest, an unsupervised machine learning algorithm designed to identify unusual observations in a dataset.

The workflow includes:

1. Loading the network traffic dataset
2. Exploring the dataset
3. Cleaning the data
4. Removing unsuitable columns
5. Selecting relevant network traffic features
6. Converting features into numerical form
7. Scaling the features
8. Training an Isolation Forest model
9. Predicting normal and anomalous traffic
10. Calculating anomaly scores
11. Evaluating model performance
12. Visualizing the results
13. Saving the trained model and scaler

---

🧠 Algorithm Used

Isolation Forest

Isolation Forest is an anomaly detection algorithm that works by isolating unusual observations from the rest of the dataset.

Instead of trying to learn every possible type of attack, the model identifies traffic that behaves differently from the majority of network traffic.

In this project, the model is configured with:

IsolationForest(
    n_estimators=100,
    contamination=0.05,
    random_state=42
)

- n_estimators = 100 — Number of trees used by the model.
- contamination = 0.05 — Estimated proportion of anomalies.
- random_state = 42 — Makes the results reproducible.

---

📊 Dataset

The project expects a CSV file named:

network_traffic.csv

The dataset contains network-flow information along with a "Label" column used for evaluating the anomaly detection results.

The project removes identifying or unsuitable fields such as:

- "Flow ID"
- "Source IP"
- "Destination IP"
- "Timestamp"

The model uses selected network traffic features including:

- Flow Duration
- Total Forward Packets
- Total Backward Packets
- Total Length of Forward Packets
- Total Length of Backward Packets
- Flow Bytes/s
- Flow Packets/s
- Forward Packet Length Mean
- Backward Packet Length Mean

---

🛠️ Technologies Used

- Python
- Pandas — Data manipulation and preprocessing
- NumPy — Numerical operations
- Scikit-learn — Machine learning and preprocessing
- Matplotlib — Data visualization
- Seaborn — Statistical visualization
- Joblib — Saving and loading the trained model

---

📁 Project Structure

network-traffic-anomaly-detection/
│
├── anomaly_detection.ipynb
├── network_traffic.csv
├── README.md
│
├── model/
│   ├── isolation_forest.pkl
│   └── scaler.pkl
│
└── requirements.txt

File Description

File/Folder| Description
"anomaly_detection.ipynb"| Main Google Colab/Jupyter Notebook
"network_traffic.csv"| Network traffic dataset
"model/"| Contains saved machine learning files
"isolation_forest.pkl"| Trained Isolation Forest model
"scaler.pkl"| Saved StandardScaler
"requirements.txt"| Required Python libraries
"README.md"| Project documentation

---

🔄 Project Workflow

Network Traffic Dataset
          ↓
     Data Cleaning
          ↓
 Remove Unwanted Columns
          ↓
   Select Features
          ↓
 Convert to Numeric Data
          ↓
    Standard Scaling
          ↓
   Isolation Forest
          ↓
    Anomaly Detection
          ↓
 Normal / Anomaly
          ↓
 Evaluation & Visualization
          ↓
 Save Model + Scaler

---

🧹 Data Preprocessing

Before training the model, the dataset is cleaned.

The project:

- Removes duplicate records
- Replaces infinite values
- Checks for missing values
- Removes rows containing missing values
- Separates labels from input features
- Rem
