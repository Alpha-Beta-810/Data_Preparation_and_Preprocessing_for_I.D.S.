# 🛡️ AI-Based Network Intrusion Detection System (IDS)

This project implements an AI-based Network Intrusion Detection System (IDS) using the NSL-KDD dataset. It preprocesses network traffic data, balances the dataset using SMOTE, trains multiple machine learning classifiers, and compares their performance using standard evaluation metrics.

---

## 📖 Overview

Cyberattacks continue to evolve, making traditional signature-based detection systems insufficient against modern threats. This project implements a **Machine Learning-based Intrusion Detection System (IDS)** capable of identifying different types of network attacks by analyzing network traffic features.

The system preprocesses raw network data, handles class imbalance using **SMOTE**, trains multiple machine learning models, and evaluates their effectiveness using standard performance metrics.

---

## ✨ Features

- 🛡️ Detects malicious network traffic using Machine Learning
- 📊 Uses the benchmark **NSL-KDD** intrusion detection dataset
- 🤖 Implements multiple ML algorithms:
  - Logistic Regression
  - Decision Tree
  - Support Vector Machine (SVM)
- ⚖️ Handles imbalanced attack classes using **SMOTE**
- 🧹 Data preprocessing including:
  - Duplicate removal
  - Missing value handling
  - Label Encoding
  - Feature Scaling
- 📈 Evaluates models using:
  - Accuracy
  - Precision
  - Recall
  - F1-Score
- 🔍 Compares multiple classifiers for intrusion detection performance
- 🚀 Clean and beginner-friendly implementation
- 💻 Built entirely in Python

---

## 🧠 Machine Learning Pipeline

**1. Dataset Loading**

Uses **NSL-KDD 20% dataset**  
Reads data using **Pandas**

```python
###Python
pd.read_csv("KDDTrain+_20Percent.txt")
```

---

**2. Feature Engineering**

The code manually assigns all **43 feature names**, including:

• duration
• protocol_type
• service
• flag
• src_bytes
• dst_bytes
• ...
• label
• difficulty

Then removes:

```
difficulty
```

since it isn't needed for prediction.

**3. Data Cleaning**

Performs:

• Duplicate removal
• Missing value removal

```
drop_duplicates()
dropna()
```

**4. Encoding**

Uses **LabelEncoder** for:

• protocol_type  
• service  
• flag

Also converts attack labels into numeric classes.

**5. Rare Class Removal**

Very small classes (<6 samples) are removed before SMOTE.

Reason:

SMOTE cannot generate synthetic samples if a class has too few observations.

**6. Normalization**

Uses
---
MinMaxScaler

which scales every feature into
---
0 → 1

This is especially beneficial for SVM.

**7. Class Balancing**

Uses

---

**Python**
SMOTE(random_state=42)

Advantages:

• Balances minority attacks
• Prevents model bias
• Improves Recall

**8. Train-Test Split**

---

70% Training
30% Testing

**9. Models Used**

**Logistic Regression**

Pros:

•Fast
•Interpretable
•Good baseline

**Decision Tree**

Pros:

• Handles nonlinear patterns
• Easy visualization
• Little preprocessing required

**Support Vector Machine (SVM)**

Pros:

• High accuracy
• Strong for complex boundaries
• Works well after scaling

## 📂 Project Structure

```
AI-Based-Network-Intrusion-Detection-System/
│
├── intrusion_det.py                 # Main IDS python implementation
├── intrusion_det_python_code.txt    # Duplicate/text version of the Python code
├── KDDTrain+.txt                    # Full NSL-KDD training dataset
├── KDDTrain+_20Percent.txt          # 20% subset used for training
├── Data Preparation and Preprocessing
    for Intrusion Detection.pdf      # Research-paper styled assignment report
└── README.md
```

---

## ⚙️ Technologies Used

| Technology | Purpose |
|------------|---------|
| Python | Programming Language |
| Pandas | Data Processing |
| NumPy | Numerical Computing |
| Scikit-learn | Machine Learning |
| Imbalanced-learn | SMOTE Oversampling |

---

## 📊 Dataset

This project uses the **NSL-KDD** dataset, an improved version of the KDD Cup 1999 dataset developed for intrusion detection research.

The dataset contains network traffic records categorized as:

- ✅ Normal
- 🚨 DoS Attacks
- 🎯 Probe Attacks
- 🔓 R2L Attacks
- ⚠️ U2R Attacks

---

## 🔄 Project Workflow

```text
Load Dataset
      │
      ▼
Assign Feature(Column) Names
      │
      ▼
Remove "difficulty" column
      │
      ▼
Data Cleaning
      │
      ▼
Encode Categorical Features
      │
      ▼
Encode Attack Labels
      │
      ▼
Remove Rare Classes
      │
      ▼
Normalize Features
      │
      ▼
Balance Dataset (SMOTE)
      │
      ▼
Train-Test Split
      │
      ▼
Train ML Models
      │
      ▼
Performance Evaluation
```

---

## 🤖 Machine Learning Models

### Logistic Regression

- Fast baseline classifier
- Suitable for linear decision boundaries

---

### Decision Tree

- Easy to interpret
- Handles nonlinear relationships

---

### Support Vector Machine (SVM)

- Effective for high-dimensional data
- Excellent classification capability after feature scaling

---

## 📈 Evaluation Metrics

The models are evaluated using:

- 🎯 Accuracy
- 📌 Precision
- 🔄 Recall
- ⚖️ F1-Score

These metrics help compare the effectiveness of each classifier in detecting network intrusions.

with

---
average="macro"

which treats every attack class equally.

---

## 🚀 Installation

### Clone the repository

```bash
git clone https://github.com/your-username/AI-Based-Network-Intrusion-Detection-System.git
```

### Navigate to the project

```bash
cd AI-Based-Network-Intrusion-Detection-System
```

### Install dependencies

```bash
pip install pandas numpy scikit-learn imbalanced-learn
```

---

## ▶️ Running the Project

Execute the Python script:

```bash
python intrusion_det.py
```

The program will:

- Load the NSL-KDD dataset
- Preprocess the data
- Balance the dataset using SMOTE
- Train multiple machine learning models
- Evaluate each classifier
- Display performance metrics

---

## 📊 Sample Output

```
Model                  Accuracy

Logistic Regression      XX.XX%

Decision Tree            XX.XX%

Support Vector Machine   XX.XX%
```

Additional metrics include:

- Precision
- Recall
- F1-Score

---

## 🎯 Applications

- Enterprise Network Security
- Intrusion Detection Systems
- Security Research
- Cybersecurity Education
- Threat Detection
- Machine Learning Demonstrations

---

## 📌 Future Improvements

- 🌲 Random Forest Classifier
- 🚀 XGBoost & LightGBM
- 📉 Feature Selection
- 📊 Confusion Matrix Visualization
- 📈 ROC Curve & AUC Analysis
- 🔍 Hyperparameter Tuning
- 📂 Support for Real-Time Network Traffic
- 🌐 Interactive Dashboard
- 🧠 Deep Learning Models (LSTM/CNN)

---

## 🤝 Contributing

Contributions are welcome!

If you'd like to improve this project:

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push your branch
5. Open a Pull Request

---

## 📜 License

This project is intended for **educational and research purposes**.

---

## 👨‍💻 Author

**Sarat Devarakonda**

If you found this project useful, consider giving it a ⭐ on GitHub!

---

## ⭐ Support

If you like this project:

⭐ Star the repository

🍴 Fork the project

🛠️ Contribute improvements

📢 Share it with others
