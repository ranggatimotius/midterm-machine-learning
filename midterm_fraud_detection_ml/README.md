# 🔐 Midterm — Machine Learning Fraud Detection

## 👤 Identitas

| | |
|---|---|
| **Nama** | Rangga Timotius |
| **NIM** | 101032300137 |
| **Kelas** | TK46GAB |

---

## 📌 Tujuan Repository

Repository ini berisi implementasi end-to-end **fraud detection pipeline** menggunakan algoritma Machine Learning untuk memprediksi probabilitas suatu transaksi online adalah fraud (`isFraud`).

---

## 📂 Gambaran Proyek

Dataset berisi transaksi e-commerce dengan fitur seperti jumlah transaksi, informasi kartu, alamat, dan lainnya. Model dilatih untuk memprediksi probabilitas fraud — sebuah masalah **klasifikasi biner yang imbalanced** (hanya 3.50% fraud).

### Dataset
| File | Ukuran | Deskripsi |
|---|---|---|
| `train_transaction.csv` | 590.540 baris × 394 kolom | Data training berlabel (`isFraud` = 0/1) |
| `test_transaction.csv` | 506.691 baris × 393 kolom | Data test tanpa label — untuk prediksi submission |

> 📌 Dataset tidak di-upload ke repo karena ukurannya 500MB+. Simpan di Google Drive dan sesuaikan `DRIVE_PATH` di notebook.

---

## 🤖 Model yang Digunakan

| Model | Deskripsi |
|---|---|
| **Logistic Regression** | Baseline klasifikasi linear dengan `class_weight='balanced'` |
| **Random Forest** | Ensemble Decision Tree, `n_estimators=100`, `max_depth=10` |
| **XGBoost** | Gradient boosting, `n_estimators=200`, `learning_rate=0.05` |
| **XGBoost (Tuned)** | XGBoost setelah `RandomizedSearchCV` — **model terbaik** |

---

## 📊 Hasil Evaluasi Model

| Model | AUC-ROC | F1-Fraud | Avg Precision | Waktu |
|---|---|---|---|---|
| Logistic Regression | 0.6789 | 0.0974 | 0.0688 | 130.7s |
| Random Forest | 0.8712 | 0.4574 | 0.4842 | 280.2s |
| XGBoost | 0.8968 | 0.5349 | 0.5471 | 54.8s |
| **XGBoost (Tuned)** | **0.9549** | **0.6743** | **0.7573** | — |

### Best Params (XGBoost Tuned)
```
subsample: 0.8 | n_estimators: 300 | min_child_weight: 1
max_depth: 8   | learning_rate: 0.1 | colsample_bytree: 0.8
```

### Confusion Matrix (XGBoost Tuned — Validation Set 118.108 baris)
|  | Prediksi Normal | Prediksi Fraud |
|---|---|---|
| **Aktual Normal** | 113.728 | 247 |
| **Aktual Fraud** | 1.905 | 2.228 |

> Precision Fraud: **90%** — Recall Fraud: **54%** — F1 Fraud: **0.67**

---

## 📈 Metrik Evaluasi

| Metrik | Keterangan |
|---|---|
| **AUC-ROC ↑** | Metrik utama — mengukur kemampuan model membedakan fraud vs normal |
| **F1-Score (Fraud)** | Harmonic mean precision & recall untuk kelas fraud |
| **Precision** | Dari prediksi fraud, berapa yang benar-benar fraud |
| **Recall** | Dari semua fraud, berapa yang berhasil terdeteksi |
| **Average Precision** | Area under Precision-Recall curve |

> ⚠️ Accuracy **tidak digunakan** karena dataset sangat imbalanced — model bisa dapat accuracy tinggi hanya dengan selalu prediksi "Normal".

---

## ⚖️ Penanganan Class Imbalance

- **SMOTE** diterapkan pada training set → data seimbang 455.902 per kelas (dari 3.5% menjadi 50/50)
- `scale_pos_weight` pada XGBoost untuk bobot kelas otomatis
- Split train/val dilakukan **sebelum** SMOTE untuk menghindari data leakage

---

## ⚡ Optimasi RAM (Memory-Efficient)

| Teknik | Dampak |
|---|---|
| `dtype downcasting` float64→float32 | 756 MB → 256 MB (hemat **66%**) |
| `usecols` — load kolom terpilih saja | 394 kolom → 168 kolom |
| `del` + `gc.collect()` setelah pakai | Bebaskan RAM segera |
| `tree_method='hist'` pada XGBoost | Training lebih hemat memori |

---

## 🗂️ Navigasi Repository

```
midterm-machine-learning/
│
├── 📓 midterm_fraud_detection_ML.ipynb  ← Notebook utama (jalankan di Google Colab)
├── 📄 submission_fraud.csv              ← Output prediksi (506.691 baris, 8.040 fraud)
└── 📄 README.md                         ← Dokumentasi ini
```

---

## 🚀 Cara Menjalankan (Google Colab)

1. **Upload dataset ke Google Drive:**
   - `train_transaction.csv`
   - `test_transaction.csv`

2. **Buka notebook di Google Colab** → [colab.research.google.com](https://colab.research.google.com)

3. **Sesuaikan path** di cell pertama:
   ```python
   DRIVE_PATH = '/content/drive/MyDrive/'  # ganti jika di subfolder
   ```

4. **Jalankan semua cell** (`Runtime → Run all`) — estimasi **±30 menit**

5. **Download notebook dengan output** → `File → Download → Download .ipynb`

---

## 🔄 Alur Pipeline

```
Load Data (Google Drive, memory-efficient)
    → EDA (distribusi fraud 3.5%, missing values, TransactionAmt)
        → Preprocessing (drop >70% missing, Label Encoding, Median Imputation, log transform)
            → SMOTE (3.5% → 50/50 balance)
                → Training: Logistic Regression | Random Forest | XGBoost
                    → Hyperparameter Tuning (RandomizedSearchCV, 15 iter, 3-fold CV)
                        → Evaluasi (ROC Curve, Confusion Matrix, Feature Importance)
                            → Prediksi 506.691 test transactions → submission_fraud.csv
```
