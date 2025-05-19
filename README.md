# Klasifikasi Keberadaan Marga per Sub-etnis Batak

<div align="center">

![Python](https://img.shields.io/badge/python-v3.8+-blue.svg)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-orange.svg)
![Pandas](https://img.shields.io/badge/pandas-1.3+-green.svg)
![Status](https://img.shields.io/badge/status-completed-success.svg)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

</div>

<p align="center">
  <img src="[https://upload.wikimedia.org/wikipedia/commons/thumb/d/d5/Batak_Toba_traditional_house_symbolizing_the_cosmos.jpg/640px-Batak_Toba_traditional_house_symbolizing_the_cosmos.jpg](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b7/Ahnenhaus_der_Batak_Toba.jpg/500px-Ahnenhaus_der_Batak_Toba.jpg)" alt="Batak Traditional House" width="400"/>
</p>

<p align="center">
  <b>Multi-label classification untuk mengidentifikasi keberadaan marga pada 6 sub-etnis Batak</b>
</p>

---

## 📑 Daftar Isi
- [Tentang Proyek](#-tentang-proyek)
- [Dataset](#-dataset)
- [Metodologi](#-metodologi)
- [Hasil & Performa](#-hasil--performa)
- [Cara Penggunaan](#-cara-penggunaan)
- [Reproduksi](#-reproduksi)
- [Kesimpulan & Insight](#-kesimpulan--insight)
- [Pengembangan Masa Depan](#-pengembangan-masa-depan)
- [Kontribusi](#-kontribusi)
- [Referensi](#-referensi)

---

## 🎯 Tentang Proyek

Proyek ini mengembangkan model machine learning untuk mengklasifikasikan keberadaan marga Batak pada 6 sub-etnis: **Angkola**, **Karo**, **Mandailing**, **Pakpak**, **Simalungun**, dan **Toba**.

Model ini mengklasifikasikan apakah suatu marga ada ("Y") atau tidak ada ("T") pada masing-masing sub-etnis Batak berdasarkan nama marga dan deskripsinya. Ini adalah masalah **multi-label classification**, di mana setiap marga dapat termasuk dalam satu atau lebih sub-etnis.

### 🛠️ Teknologi

<div align="center">

| Teknologi | Kegunaan |
|-----------|----------|
| Python 3.8+ | Bahasa pemrograman utama |
| scikit-learn | Library machine learning |
| pandas | Manipulasi dan analisis data |
| matplotlib & seaborn | Visualisasi data |
| Google Colab | Environment eksekusi |
| joblib | Serialisasi model |

</div>

---

## 📊 Dataset

Dataset berisi **349 marga Batak** dengan distribusi yang tidak seimbang (imbalanced) pada setiap sub-etnis.

### Struktur Data

| Kolom | Deskripsi |
|-------|-----------|
| `"No (Text)"` | Nomor urut marga |
| `"Marga (Text)"` | Nama marga Batak |
| `"Marga (Link)"` | Link referensi marga |
| `"Angkola (Text)"` | Label keberadaan di sub-etnis Angkola ("Y"/"T") |
| `"Karo (Text)"` | Label keberadaan di sub-etnis Karo ("Y"/"T") |
| `"Mandailing (Text)"` | Label keberadaan di sub-etnis Mandailing ("Y"/"T") |
| `"Pakpak (Text)"` | Label keberadaan di sub-etnis Pakpak ("Y"/"T") |
| `"Simalungun (Text)"` | Label keberadaan di sub-etnis Simalungun ("Y"/"T") |
| `"Toba (Text)"` | Label keberadaan di sub-etnis Toba ("Y"/"T") |
| `"Keterangan (Text)"` | Keterangan tambahan |
| `"Description"` | Deskripsi lengkap marga |

### Distribusi Label

<div align="center">

| Sub-etnis | Label "Y" | Label "T" | % "Y" |
|-----------|-----------|-----------|-------|
| Angkola | 19 | 330 | 5.4% |
| Karo | 76 | 273 | 21.8% |
| Mandailing | 22 | 327 | 6.3% |
| Pakpak | 48 | 301 | 13.8% |
| Simalungun | 62 | 287 | 17.8% |
| Toba | 188 | 161 | 53.9% |

</div>

---

## 🔍 Metodologi

### 1. Persiapan Data

- **Data Cleaning**
  - Pengecekan nilai kosong dan duplikat (tidak ditemukan)
  - Standardisasi teks (lowercase, strip whitespace)
  - Validasi konsistensi label ("Y"/"T")

- **Analisis Distribusi**
  - Identifikasi imbalance pada setiap sub-etnis
  - Visualisasi distribusi label

- **Split Data**
  - 80% training, 20% testing
  - Stratified split berdasarkan label "Toba (Text)"

### 2. Feature Engineering

- **Label Encoding**
  - Konversi "Y"/"T" ke nilai biner 1/0
  - Pembuatan multi-label target

- **Text Vectorization**
  - TF-IDF dengan ngram_range=(1,2)
  - max_features=3000 (hasil tuning)

- **Feature Combination**
  - Gabungan nama marga dan deskripsi sebagai fitur utama
  - Format: `"Marga (Text)" + " " + "Description"`

### 3. Model Development

- **Algoritma yang Diuji**
  - Random Forest (terbaik)
  - Linear SVC
  - Logistic Regression

- **Pipeline**
  - TF-IDF Vectorizer + MultiOutputClassifier
  - Preprocessing dan prediksi dalam satu pipeline

- **Hyperparameter Tuning**
  - GridSearchCV dengan 3-fold cross-validation
  - Parameter: n_estimators, max_depth, max_features

---

## 📈 Hasil & Performa

### Model Terbaik

Random Forest dengan fitur gabungan (nama marga + deskripsi) dan parameter:
- n_estimators: 100
- max_depth: None (tidak dibatasi)
- max_features (TF-IDF): 3000

### Metrik Performa

<div align="center">

| Sub-etnis   | Precision | Recall | F1-score | Support |
|-------------|-----------|--------|----------|---------|
| Angkola     | 0.00      | 0.00   | 0.00     | 1       |
| Karo        | 0.61      | 0.70   | 0.65     | 20      |
| Mandailing  | 1.00      | 0.50   | 0.67     | 2       |
| Pakpak      | 0.67      | 0.29   | 0.40     | 7       |
| Simalungun  | 0.83      | 0.31   | 0.45     | 16      |
| Toba        | 0.89      | 0.89   | 0.89     | 38      |
| **Micro avg**    | **0.79**  | **0.67** | **0.72** | **84**  |
| **Macro avg**    | **0.67**  | **0.45** | **0.51** | **84**  |
| **Weighted avg** | **0.79**  | **0.67** | **0.70** | **84**  |

</div>

---


## 🚀 Cara Penggunaan

### Instalasi Dependencies

```bash
pip install scikit-learn pandas matplotlib seaborn joblib
```

### Load Model & Prediksi

```python
import joblib

# Load model
model = joblib.load('marga_classifier_pipeline.pkl')

# Format input: "nama_marga deskripsi_marga"
input_data = ['"Simanjuntak" "Marga ini berasal dari daerah Toba dan tersebar luas"']

# Prediksi
predictions = model.predict(input_data)

# Konversi hasil ke format Y/T
label_cols = ['"Angkola (Text)"', '"Karo (Text)"', '"Mandailing (Text)"', 
              '"Pakpak (Text)"', '"Simalungun (Text)"', '"Toba (Text)"']

result = {}
for i, col in enumerate(label_cols):
    result[col] = "Y" if predictions[0][i] == 1 else "T"

print("Prediksi keberadaan marga per sub-etnis:")
for label, value in result.items():
    print(f"{label}: {value}")
```

### Contoh Output

```
Prediksi keberadaan marga per sub-etnis:
"Angkola (Text)": T
"Karo (Text)": T
"Mandailing (Text)": T
"Pakpak (Text)": T
"Simalungun (Text)": T
"Toba (Text)": Y
```

---

## 🔄 Reproduksi

### Di Google Colab

1. Upload file `marga_batak_optimized.csv` ke Google Drive
2. Buka notebook di Google Colab dengan badge: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
3. Mount Google Drive dan jalankan seluruh kode:

```python
from google.colab import drive
drive.mount('/content/drive')
```

### Di Lokal

1. Clone repository ini
2. Install dependencies: `pip install -r requirements.txt`
3. Jalankan `python convert-to-py.py` untuk melihat kode lengkap
4. Pastikan file `marga_batak_optimized.csv` tersedia di direktori yang sama

---

## 📝 Kesimpulan & Insight

- **Model Terbaik**: Random Forest dengan fitur gabungan (marga + deskripsi)
- **Sub-etnis Toba**: Performa sangat baik (F1-score 0.89) karena memiliki data paling seimbang
- **Sub-etnis Minoritas**: Angkola, Mandailing, dan Pakpak sulit diprediksi karena jumlah data "Y" sangat sedikit
- **Feature Engineering**: Penggabungan nama marga dengan deskripsi meningkatkan performa secara signifikan
- **Imbalance**: Mempengaruhi performa model pada beberapa sub-etnis (terutama Angkola)

---

## 🔮 Pengembangan Masa Depan

- **Mengatasi Imbalance**:
  - Implementasi teknik oversampling (SMOTE, RandomOverSampler)
  - Class weighting pada algoritma

- **Advanced Feature Engineering**:
  - Word embeddings (Word2Vec, FastText)
  - BERT atau model bahasa lain untuk fitur kontekstual

- **Data Enrichment**:
  - Pengumpulan lebih banyak data, terutama untuk sub-etnis minoritas
  - Augmentasi data teks

- **Deployment**:
  - Pengembangan API dengan FastAPI atau Flask
  - Web interface untuk prediksi interaktif

---

## 👥 Kontribusi

Kontribusi sangat diterima! Jika ingin berkontribusi:

1. Fork repository ini
2. Buat branch fitur baru (`git checkout -b feature/AmazingFeature`)
3. Commit perubahan (`git commit -m 'Add some AmazingFeature'`)
4. Push ke branch (`git push origin feature/AmazingFeature`)
5. Buka Pull Request

---

## 📚 Referensi

- Dataset: `marga_batak_optimized.csv`
- Kode: `convert-to-py.py` (konversi dari notebook Colab)
- Scikit-learn Documentation: [scikit-learn.org](https://scikit-learn.org/)
- Multi-label Classification: [Multi-label classification with scikit-learn](https://scikit-learn.org/stable/modules/multiclass.html)

---

<p align="center">
  <i>Developed with ❤️ for preserving cultural heritage through data science</i>
</p> 
