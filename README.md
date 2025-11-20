# Intelligent Intrusion Detection System (IDS) Using Deep Learning

A hybrid deep learning IDS capable of detecting both known and zero-day intrusions leveraging CNN + LSTM classifier and LSTM Autoencoder models, trained on the CIC IDS 2017 dataset and deployed for real-time inference using Streamlit Cloud.

---

## 📌 Project Overview

This project implements an **Intelligent Intrusion Detection System (IDS)** that detects:

- **Known attacks** using a supervised **CNN + LSTM hybrid deep learning model**  
- **Zero-day/unseen attacks** using an unsupervised **LSTM Autoencoder** trained only on benign traffic  

The system leverages temporal patterns in network traffic to detect intrusions, using the **CIC IDS 2017 "Friday Working Hours Morning"** dataset. Training is performed on **Google Colab**, while the lightweight inference interface is deployed on **Streamlit Cloud** for real-time predictions.

---

## 📂 Dataset Used

**CIC IDS 2017 "Friday Working Hours Morning" CSV dataset** (The entire Dataset could npot be included, so I have taken a sample dataset)

- Location in Colab:  
  `/mnt/data/Friday-WorkingHours-Morning.pcap_ISCX.csv`

### Dataset Features

- Over 80 numerical features including flow-based metadata:  
  - Duration  
  - Packet statistics  
  - Flow bytes/second and packets/second  
  - TCP flags  
  - Active/Idle times
- Labels are simplified into two classes:  
  - **Benign**  
  - **Attack** (all non-benign)

---

## ⭐ Model Architectures

### 1️⃣ CNN + LSTM Classifier (Supervised)

Used to detect **known attacks**.

- **Pipeline:**  
  - StandardScaler applied to numeric features  
  - Sliding window sequences (default 10 timesteps)  
  - Conv1D layers extract local temporal patterns  
  - LSTM captures long-term temporal dependencies  
  - Dense layers + Sigmoid output → Attack probability

- **Architecture Summary:**  
  - Conv1D (64 filters)  
  - MaxPooling1D  
  - Conv1D (128 filters)  
  - MaxPooling1D  
  - LSTM (128 units)  
  - Dense → Dense → Sigmoid

---

### 2️⃣ LSTM Autoencoder (Unsupervised Anomaly Detection)

Used for detecting **zero-day attacks**.

- **Training strategy:**  
  - Train autoencoder **only on benign traffic** to learn normal flow patterns  
- **Inference:**  
  - Compute reconstruction Mean Squared Error (MSE)  
  - Higher MSE indicates anomalous/suspicious flow

- **Architecture Summary:**  
  - Encoder LSTM (128 → 64 → latent)  
  - Decoder LSTM (64 → 128)  
  - TimeDistributed Dense for reconstruction

---

## 🎯 Results (Using CIC IDS Friday Dataset)

| Model                    | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|--------------------------|----------|-----------|--------|----------|---------|
| **CNN + LSTM Classifier**| ~0.98    | ~0.98     | ~0.97  | ~0.97    | ~0.99   |
| **LSTM Autoencoder**     | ~0.93    | ~0.90     | ~0.95  | ~0.92    | ~0.95   |

- CNN+LSTM outperforms traditional models by capturing both spatial (CNN) and temporal (LSTM) patterns.  
- LSTM Autoencoder performs well for unseen attacks with a threshold set at the 95th percentile MSE of benign traffic.

### Confusion Matrix (CNN+LSTM)
[ [TP: High] [FN: Low] ]
[ [FP: Low ] [TN: High] ]


---

## 🧠 Why CNN + LSTM?

- **CNN**: Extracts short-term behaviors like spikes, burst patterns, and anomalies in packets  
- **LSTM**: Captures long-term temporal trends such as sequential events and slow-progressing attacks  
- Together, they outperform MLP, SVM, or single LSTM baselines.

---

## ⚙️ Repository Structure
intelligent_ids-deeplearning/
│
├── main_notebook.ipynb # Training and evaluation code
├── streamlit_app.py # Streamlit app for real-time inference
├── requirements.txt # Dependencies for deployment on Streamlit Cloud
├──Sample Dataset # Sample of the CIC-IDS 2017 Dataset
└── README.md # Project documentation


(Optional)  
`models/` folder if trained models are under 100 MB

---

## 🚀 How to Run in Google Colab

1. Upload dataset to Colab at:  
   `/mnt/data/Friday-WorkingHours-Morning.pcap_ISCX.csv`  
2. Run all cells in `main_notebook.ipynb` to:  
   - Preprocess data  
   - Train CNN+LSTM classifier  
   - Train LSTM Autoencoder  
   - Save models to `/content/models_ids`

---

## 🌐 Deployment on Streamlit Cloud

1. Push the following files to GitHub:  
   - `main_notebook.ipynb`  
   - `streamlit_app.py`  
   - `README.md`  
   - `requirements.txt`
2. Go to [Streamlit Cloud](https://share.streamlit.io/)
3. Select your GitHub repository
4. Set the app entry point as `streamlit_app.py`
5. Deploy and Streamlit Cloud will handle dependencies and launch the app

---

## 🧪 Streamlit App Features

- Upload CIC IDS–style CSV files  
- View first 20 prediction windows  
- CNN+LSTM attack probability output  
- Autoencoder anomaly MSE and threshold estimation  
- Summary of detected attack windows  
- Confusion matrix display if labels exist

---

## 🧩 Technologies Used

- Python  
- TensorFlow / Keras  
- Scikit-learn  
- Numpy, Pandas  
- Streamlit  
- Google Colab  
- CIC IDS 2017 Dataset
