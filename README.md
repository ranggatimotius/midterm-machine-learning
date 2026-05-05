# 🎵 Midterm — Machine Learning Regression

## 👤 Identitas

| | |
|---|---|
| **Nama** | Rangga Timotius |
| **NIM** | 101032300137 |
| **Kelas** | TK46GAB |

---

## 📌 Tujuan Repository

Repository ini berisi implementasi end-to-end **regression pipeline** menggunakan algoritma Machine Learning untuk memprediksi **tahun rilis lagu** berdasarkan fitur-fitur audio dari dataset *Million Song Dataset* (subset).

---

## 📂 Gambaran Proyek

Setiap lagu direpresentasikan sebagai vektor numerik dari 90 fitur audio (seperti timbre dan karakteristik sinyal musik). Model dilatih untuk memprediksi tahun rilis lagu (1922–2011) dari fitur-fitur tersebut — sebuah masalah **regresi kontinu**.

### Dataset
- **File:** `midterm-regresi-dataset.csv`
- **Ukuran:** 515.345 baris × 91 kolom
- **Target:** Kolom pertama — tahun rilis lagu (integer)
- **Fitur:** 90 kolom numerik (`feature_1` hingga `feature_90`)
- **Missing Values:** Tidak ada

---

## 🤖 Model yang Digunakan

| Model | Deskripsi |
|---|---|
| **Linear Regression** | Baseline model regresi linear |
| **Ridge Regression** | Linear Regression dengan regularisasi L2 |
| **Lasso Regression** | Linear Regression dengan regularisasi L1 + feature selection |
| **Decision Tree Regressor** | Model pohon keputusan untuk pola non-linear |
| **Random Forest Regressor** | Ensemble dari banyak Decision Tree (model terbaik) |
| **Gradient Boosting Regressor** | Boosting ensemble untuk performa tinggi |

---

## 📊 Metrik Evaluasi

| Metrik | Keterangan |
|---|---|
| **MSE** | Mean Squared Error — rata-rata kuadrat error |
| **RMSE** | Root MSE — interpretasi dalam satuan tahun |
| **MAE** | Mean Absolute Error — rata-rata selisih absolut |
| **R²** | Koefisien determinasi — seberapa baik model menjelaskan variansi data |

---

## 🗂️ Navigasi Repository

```
midterm-machine-learning/
│
├── 📓 midterm_regression_ml.ipynb   ← Notebook utama (jalankan ini)
├── 📄 midterm-regresi-dataset.csv   ← Dataset (letakkan di folder yang sama)
├── 🖼️ target_distribution.png       ← Grafik distribusi target (auto-generated)
├── 🖼️ correlation_heatmap.png       ← Heatmap korelasi fitur (auto-generated)
├── 🖼️ model_comparison.png          ← Grafik perbandingan model (auto-generated)
├── 🖼️ pred_vs_actual.png            ← Scatter plot prediksi vs aktual (auto-generated)
├── 🖼️ feature_importance.png        ← Feature importance chart (auto-generated)
└── 📄 README.md                     ← Dokumentasi ini
```

---

## 🚀 Cara Menjalankan

1. **Clone repository ini:**
   ```bash
   git clone https://github.com/<username>/midterm-machine-learning.git
   cd midterm-machine-learning
   ```

2. **Siapkan environment Python:**
   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn jupyter
   ```

3. **Letakkan dataset** `midterm-regresi-dataset.csv` di folder yang sama dengan notebook.

4. **Jalankan notebook:**
   ```bash
   jupyter notebook midterm_regression_ml.ipynb
   ```

5. Jalankan semua cell secara berurutan dari atas ke bawah.

---

## 🔄 Alur Pipeline

```
Load Data → EDA → Preprocessing → Train/Test Split
    → Model Training (6 algoritma)
        → Hyperparameter Tuning (GridSearchCV)
            → Evaluasi & Visualisasi → Kesimpulan
```

---

## 📝 Catatan

- Notebook menggunakan **50.000 sampel acak** dari 515K+ data agar training lebih cepat. Untuk performa maksimal, ubah `SAMPLE_SIZE` di notebook.
- Model terbaik secara keseluruhan adalah **Random Forest Regressor** setelah hyperparameter tuning.
- Semua grafik visualisasi tersimpan otomatis sebagai file `.png`.
