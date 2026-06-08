# Installation and Usage Guide

## Download the Project

Clone the repository:

```bash
git clone https://github.com/SamuelKendrik/credit-card-fraud-detection.git
cd credit-card-fraud-detection
```

Or download the ZIP file from GitHub and extract it.

---

## Install Requirements

Create a virtual environment (recommended):

```bash
python -m venv venv
```

Activate the virtual environment:

### Windows

```bash
venv\Scripts\activate
```

### Linux / macOS

```bash
source venv/bin/activate
```

Install all required packages:

```bash
pip install -r requirements.txt
```

---

## Download the Dataset

The dataset is not included in this repository because of GitHub's file size limitations.

Download the dataset from:

https://www.kaggle.com/mlg-ulb/creditcardfraud

After downloading, place the file inside the dataset folder:

```text
dataset/
└── creditcard.csv
```

Final project structure:

```text
FraudDetection/
│
├── dataset/
│   └── creditcard.csv
├── models/
├── app.py
├── train_models.py
├── requirements.txt
└── README.md
```

---

## Training the Models

If the trained models do not exist, run:

```bash
python train_models.py
```

This will:

1. Load the dataset.
2. Split data into training and testing sets.
3. Scale all features using StandardScaler.
4. Train all machine learning models.
5. Save trained models into the models folder.

Generated files:

```text
models/
├── logistic_regression.pkl
├── naive_bayes.pkl
├── random_forest.pkl
├── svm.pkl
├── xgboost.pkl
└── scaler.pkl
```

---

## Running the Application

Launch Streamlit:

```bash
streamlit run app.py
```

The application will open in your browser.

---

# Understanding Transaction Input

The application expects exactly 30 numerical values separated by commas.

Input format:

```text
Time,V1,V2,V3,V4,V5,V6,V7,V8,V9,V10,V11,V12,V13,V14,V15,V16,V17,V18,V19,V20,V21,V22,V23,V24,V25,V26,V27,V28,Amount
```

### Feature Explanation

| Feature  | Description                                                 |
| -------- | ----------------------------------------------------------- |
| Time     | Seconds elapsed since the first transaction in the dataset  |
| V1 - V28 | Anonymized PCA-transformed features provided by the dataset |
| Amount   | Transaction amount                                          |

Because the original transaction information contains sensitive financial data, the dataset creators transformed most features into V1-V28 using Principal Component Analysis (PCA). Their exact meanings are intentionally hidden.

---

# Example Inputs

## 1. Very Normal Transaction

```text
50000,0.12,-0.08,0.15,-0.05,0.07,-0.03,0.11,-0.04,0.06,-0.02,0.09,-0.07,0.03,-0.08,0.05,-0.04,0.02,-0.01,0.06,-0.03,0.04,-0.02,0.01,-0.05,0.03,-0.02,0.01,-0.01,89.50
```

Characteristics:

* Small feature values near zero.
* No extreme deviations.
* Moderate transaction amount.
* Usually classified as a legitimate transaction.

---

## 2. Borderline Transaction

```text
65000,-0.75,0.81,-0.62,1.25,-0.58,0.41,-0.95,0.36,-0.84,-0.71,1.15,-0.98,0.22,-1.65,0.35,-0.72,-1.08,0.16,0.19,0.08,0.32,-0.14,-0.27,-0.11,0.06,0.12,0.08,-0.03,425.75
```

Characteristics:

* Several features have larger deviations.
* Transaction amount is relatively high.
* Models may disagree on the classification.
* Useful for testing ensemble voting behavior.

---

## 3. Strong Fraud Example

```text
35000,-2.8,2.1,-1.9,3.4,-0.8,-1.2,-2.4,1.3,-2.2,-2.0,2.8,-2.3,-0.5,-3.8,0.4,-1.2,-2.1,0.1,0.3,0.1,0.5,-0.2,-0.4,0.2,0.1,0.2,0.3,-0.1,0
```

Characteristics:

* Multiple extreme feature values.
* Strong deviations from normal transactions.
* High probability of fraud detection.

---

## 4. Very Strong Fraud Example

```text
406,-5.5,4.8,-4.2,6.3,-2.1,-2.8,-4.9,2.6,-5.1,-4.7,5.3,-4.8,-1.2,-7.4,1.1,-2.8,-4.5,0.2,0.6,0.2,0.9,-0.4,-0.8,0.5,0.2,0.4,0.6,-0.2,0
```

Characteristics:

* Extremely large positive and negative values.
* Highly unusual behavior compared to normal transactions.
* Usually detected as fraud by nearly all models.
* Useful for demonstrating fraud detection capabilities.

---

# Prediction Workflow

1. User enters transaction values.
2. Data is converted into a DataFrame.
3. StandardScaler transforms the input using the same scaling used during training.
4. All machine learning models generate predictions.
5. Confidence scores are calculated when available.
6. Ensemble voting combines model decisions.
7. Final result is displayed as Fraud Detected or Legitimate Transaction.


---

## Author

Samuel Kendrik

Credit Card Fraud Detection Project using Machine Learning and Streamlit.
