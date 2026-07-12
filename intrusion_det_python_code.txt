import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder, MinMaxScaler
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.svm import SVC
from imblearn.over_sampling import SMOTE

# ---------- Step 1: Load the Dataset ----------
df = pd.read_csv("KDDTrain+_20Percent.txt", header=None)

# ---------- Step 2: Assign Column Names (43 columns) ----------
column_names = [
    'duration','protocol_type','service','flag','src_bytes','dst_bytes','land','wrong_fragment',
    'urgent','hot','num_failed_logins','logged_in','num_compromised','root_shell','su_attempted',
    'num_root','num_file_creations','num_shells','num_access_files','num_outbound_cmds',
    'is_host_login','is_guest_login','count','srv_count','serror_rate','srv_serror_rate',
    'rerror_rate','srv_rerror_rate','same_srv_rate','diff_srv_rate','srv_diff_host_rate',
    'dst_host_count','dst_host_srv_count','dst_host_same_srv_rate','dst_host_diff_srv_rate',
    'dst_host_same_src_port_rate','dst_host_srv_diff_host_rate','dst_host_serror_rate',
    'dst_host_srv_serror_rate','dst_host_rerror_rate','dst_host_srv_rerror_rate','label', 'difficulty'
]
df.columns = column_names

# ---------- Step 3: Drop 'difficulty' Column ----------
df.drop('difficulty', axis=1, inplace=True)

# ---------- Step 4: Dataset Summary BEFORE Preprocessing ----------
twenty_percent_df = df.copy()  # Save raw copy
print(f"\n📁 Full dataset shape: (125973, 42)")  # Hardcoded full dataset size for context
print(f"📁 20% dataset shape: {twenty_percent_df.shape}")

print("\n🔍 First few rows of 20% dataset:")
print(twenty_percent_df.head())

# ---------- Step 5: Clean Data ----------
df.drop_duplicates(inplace=True)
df.dropna(inplace=True)

# ---------- Step 6: Encode Categorical Features ----------
categorical_cols = ['protocol_type', 'service', 'flag']
label_encoders = {}
for col in categorical_cols:
    le = LabelEncoder()
    df[col] = le.fit_transform(df[col])
    label_encoders[col] = le

# ---------- Step 7: Encode Labels ----------
le_label = LabelEncoder()
df['label'] = le_label.fit_transform(df['label'])

# ---------- Step 8: Remove Classes with Fewer Than 6 Samples ----------
min_samples_required = 6
class_counts = df['label'].value_counts()
valid_classes = class_counts[class_counts >= min_samples_required].index
dropped_classes = class_counts[class_counts < min_samples_required]

if not dropped_classes.empty:
    print("\n⚠️ SMOTE Warning: The following classes were dropped due to insufficient samples (<6):")
    print(dropped_classes)

df = df[df['label'].isin(valid_classes)]

# ---------- Step 9: Prepare Features and Labels ----------
X = df.drop('label', axis=1)
y = df['label']

# ---------- Step 10: Normalize Numerical Features ----------
scaler = MinMaxScaler()
X_scaled = pd.DataFrame(scaler.fit_transform(X), columns=X.columns)

# ---------- Step 11: Balance Dataset Using SMOTE ----------
smote = SMOTE(random_state=42)
X_balanced, y_balanced = smote.fit_resample(X_scaled, y)

# ✅ Final balanced dataset (43 columns)
final_data = X_balanced.copy()
final_data['label'] = y_balanced

# ---------- Step 12: Train/Test Split ----------
X_train, X_test, y_train, y_test = train_test_split(X_balanced, y_balanced, test_size=0.3, random_state=42)

# ---------- Step 13: Train and Evaluate Models ----------
models = {
    "Logistic Regression": LogisticRegression(max_iter=1000),
    "Decision Tree": DecisionTreeClassifier(),
    "SVM": SVC()
}

results = []

print("\n🔍 Individual Model Metrics:")
for name, model in models.items():
    model.fit(X_train, y_train)
    preds = model.predict(X_test)

    acc = accuracy_score(y_test, preds) * 100
    prec = precision_score(y_test, preds, average='macro') * 100
    rec = recall_score(y_test, preds, average='macro') * 100
    f1 = f1_score(y_test, preds, average='macro') * 100

    print(f"\n📌 {name} Performance:")
    print(f"  - Accuracy:  {acc:.2f}%")
    print(f"  - Precision: {prec:.2f}%")
    print(f"  - Recall:    {rec:.2f}%")
    print(f"  - F1-Score:  {f1:.2f}%")

    results.append([name, round(acc, 2), round(prec, 2), round(rec, 2), round(f1, 2)])

# ---------- Step 14: Summary Table ----------
results_df = pd.DataFrame(results, columns=["Classifier", "Accuracy (%)", "Precision (%)", "Recall (%)", "F1-Score (%)"])

print("\n📊 Combined Classifier Performance Comparison:")
print(results_df.to_string(index=False))

# ✅ Final outputs:
# - twenty_percent_df → Original 20% dataset (first 5 rows printed)
# - final_data        → Processed, balanced dataset (43 columns)
# - results_df        → Model performance summary
