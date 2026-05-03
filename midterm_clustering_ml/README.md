# 💳 Midterm — Machine Learning Clustering

## 👤 Identitas

| | |
|---|---|
| **Nama** | Rangga Timotius |
| **NIM** | 101032300137 |
| **Kelas** | TK46GAB |

---

## 📌 Tujuan Repository

Repository ini berisi implementasi end-to-end **clustering pipeline** menggunakan algoritma *Unsupervised Learning* untuk mengelompokkan pelanggan berdasarkan perilaku penggunaan kartu kredit mereka.

---

## 📂 Gambaran Proyek

Dataset berisi 8.950 pelanggan dengan 17 fitur numerik yang mencakup saldo, frekuensi pembelian, cash advance, limit kredit, dan pola pembayaran. Model dilatih untuk menemukan segmentasi pelanggan secara otomatis tanpa label.

### Dataset
- **File:** `clusteringmidterm.csv`
- **Ukuran:** 8.950 baris × 18 kolom
- **Target:** Tidak ada (unsupervised)
- **Fitur utama:** BALANCE, PURCHASES, CASH_ADVANCE, CREDIT_LIMIT, PAYMENTS, PRC_FULL_PAYMENT, dll.
- **Missing Values:** CREDIT_LIMIT (1), MINIMUM_PAYMENTS (313) → diisi dengan median

---

## 🤖 Model yang Digunakan

| Model | Deskripsi |
|---|---|
| **K-Means (K=4)** | Clustering berbasis centroid — model terbaik |
| **Hierarchical / Agglomerative** | Clustering berbasis pohon (dendrogram), K=4 |
| **DBSCAN** | Clustering berbasis densitas, otomatis menemukan jumlah cluster |

---

## 📊 Metrik Evaluasi

| Metrik | Keterangan |
|---|---|
| **Silhouette Score ↑** | Seberapa baik pemisahan antar cluster (makin tinggi makin baik) |
| **Davies-Bouldin ↓** | Rasio jarak intra/inter cluster (makin kecil makin baik) |
| **Calinski-Harabasz ↑** | Rasio dispersi antar/dalam cluster (makin besar makin baik) |

### Hasil Evaluasi

| Model | Silhouette | Davies-Bouldin | Calinski-Harabasz |
|---|---|---|---|
| **K-Means (K=4)** | 0.2103 | 1.5290 | 2464.82 |
| Agglomerative (K=4) | 0.1777 | 1.6907 | 1876.04 |
| DBSCAN | 0.3907 | N/A | N/A |

---

## 🧩 Interpretasi Cluster (K-Means, K=4)

| Cluster | Segmen | Karakteristik |
|---|---|---|
| **0** | 🟢 Pengguna Aktif Moderat | Balance rendah, pembelian aktif, pembayaran rutin |
| **1** | 🔵 Pengguna Pasif | Balance rendah, pembelian minim, sedikit cash advance |
| **2** | 🔴 Cash Advance Heavy | Balance & cash advance sangat tinggi, jarang bayar penuh |
| **3** | 🟡 Big Spender | Pembelian & credit limit sangat tinggi, pembayaran besar |

---

## 🗂️ Navigasi Repository

```
midterm-machine-learning/
│
├── 📓 midterm_clustering_ml.ipynb   ← Notebook utama (jalankan ini)
├── 📄 clusteringmidterm.csv         ← Dataset (letakkan di folder yang sama)
└── 📄 README.md                     ← Dokumentasi ini
```

---

## 🚀 Cara Menjalankan

1. **Clone repository:**
   ```bash
   git clone https://github.com/<username>/midterm-machine-learning.git
   cd midterm-machine-learning
   ```

2. **Install dependencies:**
   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn scipy jupyter
   ```

3. **Letakkan dataset** `clusteringmidterm.csv` di folder yang sama.

4. **Jalankan notebook:**
   ```bash
   jupyter notebook midterm_clustering_ml.ipynb
   ```

---

## 🔄 Alur Pipeline

```
Load Data → EDA → Preprocessing (imputation + IQR clipping + scaling)
    → Elbow Method + Silhouette Score (menentukan K optimal)
        → Training: K-Means | Agglomerative | DBSCAN
            → Evaluasi & Visualisasi PCA
                → Profiling Cluster → Kesimpulan & Insight Bisnis
```
