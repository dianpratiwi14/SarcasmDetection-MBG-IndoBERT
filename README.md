# Sarcasm Detection on Public Opinion Toward the Free Nutritious Meal Program Using IndoBERT

This repository implements a **complete experimental pipeline** for sarcasm detection on Indonesian-language tweets related to the **Free Nutritious Meal Program** (Program Makan Bergizi Gratis / MBG) on social media X. The system evaluates **IndoBERT against SVM as a baseline**, and analyzes the impact of integrating a **data-driven sarcasm lexicon** on model performance, in a fully reproducible workflow.

## 🔍 Overview

Key challenges addressed:

- **Implicit and context-dependent nature of sarcasm**, making automatic detection difficult
- **Class imbalance** in the collected sarcasm dataset
- **Effectiveness of sarcasm lexicon features** across different model architectures

### Proposed Solution

**Baseline vs Transformer-based model comparison:**
- SVM (with TF-IDF / lexicon features) as baseline
- IndoBERT (fine-tuned) as the main model

**Sarcasm lexicon integration:**
- Data-driven sarcasm lexicon built from the collected corpus
- Evaluated as an additional feature for both SVM and IndoBERT

**Class imbalance handling:**
- SMOTE for SVM
- Class weighting for IndoBERT

Twelve experimental schemes evaluated across dataset variation, algorithm, and lexicon feature usage.

## 🧠 Experimental Design

### 1️⃣ Data Collection

Two datasets were used in this research:

**Dataset 1 — External Sarcasm Dataset**
- General Indonesian sarcasm dataset obtained from a prior study, *"Using LSTM for Context Based Approach of Sarcasm Detection in Twitter"*
- Total: **8,700 tweets**, with balanced class distribution (4,350 tweets per class: Sarcasm / Non-Sarcasm)

**Dataset 2 — Crawled MBG Public Opinion Dataset**
- Collected via crawling from social media X using the **Tweet-Harvest** tool
- Nine keywords used, covering general terms, exact phrases, and hashtags (e.g. `#MakanBergiziGratis`, `#ProgramMBG`, `#MBG`), filtered with `lang:id`, max 500 tweets per keyword per session
- Collected in two phases:
  - Pre-implementation phase: 1 November 2024 – 5 January 2025
  - Implementation phase: 6 January 2025 – 31 March 2025
- Crawling was split into two-week batches per phase to avoid platform rate limiting, with a random delay of 15–25 seconds between keywords
- Raw data collected: **1,633 tweets** (pre-implementation) + **3,477 tweets** (implementation) = **5,110 tweets** total, with 15 attributes per record
- Only the `full_text` attribute was used for labeling and sarcasm classification
- Reduced to **2,539 data** after deduplication and preprocessing

### 2️⃣ Labeling
- Manual annotation by two annotators
- Inter-annotator agreement: **Cohen's Kappa = 0.6811** (moderate agreement)

### 3️⃣ Model Comparison
- SVM (baseline)
- IndoBERT (fine-tuned, main model)

Both evaluated with and without sarcasm lexicon features.

## 🧱 End-to-End Pipeline

**1. Data Collection**
- Dataset 1: External Indonesian sarcasm dataset (8,700 tweets, balanced)
- Dataset 2: Crawling tweets related to Program MBG using Tweet-Harvest across two phases (pre-implementation & implementation)

**2. Data Preprocessing**

*2.1 Text Preparation*
- Text cleaning (URL, mention, hashtag, emoji, non-alphabetic character removal, repeated character reduction)
- Slang normalization
- Lowercasing
- Deduplication (by `id_str` for Dataset 2, by cleaned text for Dataset 1)
- IndoBERT: tokenization using IndoBERT tokenizer
- SVM: stopword removal (PySastrawi) + TF-IDF vectorization (max 5,000 features)

*2.2 Labelling Data*
- Manual labeling by two annotators (Sarcasm / Non-Sarcasm)
- Inter-annotator agreement evaluation using Cohen's Kappa

*2.3 Splitting Data*
- Train-test split

*2.4 Imbalanced Data Handling*
- SMOTE applied on training data (SVM)
- Class weighting applied during training (IndoBERT)

**3. Model Building**
- 3.1 SVM model building (baseline)
- 3.2 IndoBERT model building (fine-tuned)
- 3.3 Sarcasm lexicon construction (data-driven)
- 3.4 Experimental schemes design (twelve schemes: dataset variation × algorithm × lexicon feature)

**4. Analysis & Model Evaluation**
- Accuracy, Precision, Recall, F1-score
- Confusion Matrix
- Performance comparison across experimental schemes
- Temporal analysis of sarcasm proportion before and after Program MBG implementation

**5. Discussion**
- Interpretation of results and their implications

## 🏷️ Class Labels

| Label | Class Name |
|-------|------------|
| 0 | Non-Sarcasm |
| 1 | Sarcasm |

## 📊 Key Results

- SVM achieved the best performance with an **accuracy of 0.9055** and **F1-score of 0.9055**
- IndoBERT achieved an **accuracy of 0.8917** and **F1-score of 0.8947**
- Adding sarcasm lexicon features did not consistently improve performance, particularly for IndoBERT, which showed a significant performance drop after lexicon integration
- Temporal analysis shows an increase in sarcasm proportion following the implementation of Program MBG

## 📁 Repository Structure

```
├── data/
│   ├── dataset_1/
│   │   ├── raw/              → link to Dataset 1 source
│   │   └── preprocessed/     → cleaned & labeled Dataset 1
│   └── dataset_2/
│       ├── raw/              → link to raw crawled tweets
│       ├── preprocessed/     → cleaned Dataset 2
│       └── labelling/        → annotator labels & consensus files
├── src/
│   └── dataset_2_MBG/        → notebooks, numbered by pipeline order
├── output/
│   └── figures/              → all figures (English version)
├── README.md
└── requirements.txt
```

## 🚀 How to Run

> **Note:** These notebooks were developed and are intended to be run on **Google Colab**, as they use `google.colab.drive` to mount Google Drive for accessing datasets and saving model outputs. Running them outside Colab (e.g. local Jupyter Notebook) will require adjusting the file paths and removing the Drive-mounting cell.

1. Open the notebooks in `src/dataset_2_MBG/` in [Google Colab](https://colab.research.google.com/)
2. Install dependencies (first cell of each notebook, or run):
```bash
pip install -r requirements.txt
```
3. Mount your Google Drive and adjust the dataset paths to match your own Drive structure
4. Run the notebooks in numbered order (00 → 07) to execute the full pipeline, from crawling through preprocessing, labelling, splitting, model building, and evaluation

## ✨ Key Features

- Fully reproducible experimental pipeline
- Baseline (SVM) vs Transformer-based (IndoBERT) comparison
- Data-driven sarcasm lexicon integration and evaluation
- Class imbalance handling tailored per model (SMOTE / class weight)
- Temporal analysis of public sarcasm trends

## Environment
- Python 3.10
- Tested on Windows 10/11

📧 **Author:** Dian Pratiwi

📩 diapratiwi14044@gmail.com

---

# Analisis dan Deteksi Sarkasme pada Opini Publik terhadap Program Makan Bergizi Gratis Menggunakan Model IndoBERT

Repositori ini mengimplementasikan **pipeline eksperimen lengkap** untuk deteksi sarkasme pada tweet berbahasa Indonesia terkait **Program Makan Bergizi Gratis (MBG)** di media sosial X. Sistem ini mengevaluasi kinerja **IndoBERT dibandingkan dengan SVM sebagai model pembanding (baseline)**, serta menganalisis pengaruh integrasi **sarcasm lexicon berbasis data** terhadap kinerja model, dalam alur eksperimen yang reproducible.

## 🔍 Gambaran Umum

Tantangan utama yang ditangani:

- **Sifat sarkasme yang implisit dan bergantung konteks**, sehingga sulit dideteksi secara otomatis
- **Ketidakseimbangan kelas** pada dataset sarkasme yang dikumpulkan
- **Efektivitas fitur sarcasm lexicon** pada arsitektur model yang berbeda

### Solusi yang Diusulkan

**Perbandingan model baseline vs transformer-based:**
- SVM (dengan fitur TF-IDF / lexicon) sebagai baseline
- IndoBERT (fine-tuned) sebagai model utama

**Integrasi sarcasm lexicon:**
- Lexicon sarkasme dibangun secara data-driven dari korpus yang dikumpulkan
- Dievaluasi sebagai fitur tambahan pada SVM maupun IndoBERT

**Penanganan ketidakseimbangan kelas:**
- SMOTE untuk SVM
- Class weighting untuk IndoBERT

Dua belas skema eksperimen dievaluasi berdasarkan variasi dataset, algoritma, dan penggunaan fitur lexicon.

## 🧠 Desain Eksperimen

### 1️⃣ Pengumpulan Data

Penelitian ini menggunakan dua jenis dataset:

**Dataset 1 — Dataset Sarkasme Umum (Eksternal)**
- Dataset sarkasme umum berbahasa Indonesia yang diperoleh dari penelitian terdahulu berjudul *"Using LSTM for Context Based Approach of Sarcasm Detection in Twitter"*
- Total: **8.700 tweet**, dengan distribusi kelas seimbang (4.350 tweet per kelas: Sarkasme / Non-Sarkasme)

**Dataset 2 — Dataset Hasil Crawling Opini Publik terhadap MBG**
- Dikumpulkan melalui proses crawling dari media sosial X menggunakan tool **Tweet-Harvest**
- Menggunakan sembilan kata kunci yang mencakup kata kunci umum, frasa eksak, dan tagar (misalnya `#MakanBergiziGratis`, `#ProgramMBG`, `#MBG`), dengan filter `lang:id`, maksimum 500 tweet per kata kunci per sesi
- Dikumpulkan dalam dua fase:
  - Fase pra-implementasi: 1 November 2024 – 5 Januari 2025
  - Fase implementasi: 6 Januari 2025 – 31 Maret 2025
- Crawling dibagi menjadi beberapa batch dengan rentang waktu per dua minggu untuk menghindari pembatasan sistem (rate limiting), dengan jeda acak 15–25 detik di setiap pergantian kata kunci
- Data mentah yang diperoleh: **1.633 tweet** (fase pra-implementasi) + **3.477 tweet** (fase implementasi) = **5.110 tweet**, dengan 15 atribut per data
- Hanya atribut `full_text` yang digunakan untuk proses pelabelan dan klasifikasi sarkasme
- Menjadi **2.539 data** setelah deduplication dan preprocessing

### 2️⃣ Pelabelan
- Anotasi manual oleh dua anotator
- Nilai kesepakatan antar anotator: **Cohen's Kappa = 0,6811** (moderate agreement)

### 3️⃣ Perbandingan Model
- SVM (baseline)
- IndoBERT (fine-tuned, model utama)

Keduanya dievaluasi dengan dan tanpa fitur sarcasm lexicon.

## 🧱 Pipeline End-to-End

**1. Pengumpulan Data**
- Dataset 1: Dataset sarkasme umum berbahasa Indonesia (eksternal), 8.700 tweet, seimbang
- Dataset 2: Crawling tweet terkait Program MBG menggunakan Tweet-Harvest dalam dua fase (pra-implementasi & implementasi)

**2. Data Preprocessing**

*2.1 Text Preparation*
- Text cleaning (penghapusan URL, mention, hashtag, emoji, karakter non-alfabet, reduksi karakter berulang)
- Normalisasi kata tidak baku
- Lowercasing
- Deduplication (berdasarkan `id_str` untuk Dataset 2, berdasarkan teks bersih untuk Dataset 1)
- IndoBERT: tokenisasi menggunakan tokenizer IndoBERT
- SVM: stopword removal (PySastrawi) + vektorisasi TF-IDF (maksimum 5.000 fitur)

*2.2 Labelling Data*
- Pelabelan manual oleh dua anotator (Sarkasme / Non-Sarkasme)
- Evaluasi kesepakatan antar anotator menggunakan Cohen's Kappa

*2.3 Splitting Data*
- Train-test split

*2.4 Imbalanced Data Handling*
- SMOTE diterapkan pada data training (SVM)
- Class weighting diterapkan saat training (IndoBERT)

**3. Pembuatan Model**
- 3.1 Pembuatan model SVM (baseline)
- 3.2 Pembuatan model IndoBERT (fine-tuned)
- 3.3 Pembuatan lexicon sarkasme (berbasis data)
- 3.4 Perancangan skema percobaan (dua belas skema: variasi dataset × algoritma × fitur lexicon)

**4. Analisis dan Evaluasi Model**
- Accuracy, Precision, Recall, F1-score
- Confusion Matrix
- Perbandingan kinerja antar skema eksperimen
- Analisis temporal proporsi sarkasme sebelum dan sesudah implementasi Program MBG

**5. Pembahasan**
- Interpretasi hasil dan implikasinya

## 🏷️ Label Kelas

| Label | Nama Kelas |
|-------|------------|
| 0 | Non-Sarkasme |
| 1 | Sarkasme |

## 📊 Hasil Utama

- SVM memperoleh performa terbaik dengan **accuracy 0,9055** dan **F1-score 0,9055**
- IndoBERT memperoleh **accuracy 0,8917** dan **F1-score 0,8947**
- Penambahan fitur sarcasm lexicon tidak selalu meningkatkan performa, khususnya pada IndoBERT yang mengalami penurunan performa signifikan setelah integrasi lexicon
- Analisis temporal menunjukkan peningkatan proporsi sarkasme setelah implementasi Program MBG

## 📁 Struktur Repositori

```
├── data/
│   ├── dataset_1/
│   │   ├── raw/              → link sumber Dataset 1
│   │   └── preprocessed/     → Dataset 1 yang sudah dibersihkan & dilabel
│   └── dataset_2/
│       ├── raw/              → link data mentah hasil crawling
│       ├── preprocessed/     → Dataset 2 yang sudah dibersihkan
│       └── labelling/        → label annotator & file consensus
├── src/
│   └── dataset_2_MBG/        → notebook, diberi nomor sesuai urutan pipeline
├── output/
│   └── figures/              → seluruh grafik (versi Bahasa Inggris)
├── README.md
└── requirements.txt
```

## 🚀 Cara Menjalankan

> **Catatan:** Notebook ini dikembangkan dan ditujukan untuk dijalankan di **Google Colab**, karena menggunakan `google.colab.drive` untuk mount Google Drive dalam mengakses dataset dan menyimpan hasil model. Menjalankan di luar Colab (misal Jupyter Notebook lokal) memerlukan penyesuaian path file dan penghapusan cell mount Drive.

1. Buka notebook di folder `src/dataset_2_MBG/` di [Google Colab](https://colab.research.google.com/)
2. Install dependency (cell pertama tiap notebook, atau jalankan):
```bash
pip install -r requirements.txt
```
3. Mount Google Drive kamu dan sesuaikan path dataset dengan struktur Drive kamu sendiri
4. Jalankan notebook sesuai urutan nomor (00 → 07) untuk mengeksekusi seluruh pipeline, mulai dari crawling, preprocessing, pelabelan, splitting, pembuatan model, hingga evaluasi

## ✨ Fitur Utama

- Pipeline eksperimen yang sepenuhnya reproducible
- Perbandingan model baseline (SVM) vs transformer-based (IndoBERT)
- Integrasi dan evaluasi sarcasm lexicon berbasis data
- Penanganan ketidakseimbangan kelas sesuai karakteristik masing-masing model (SMOTE / class weight)
- Analisis temporal tren sarkasme publik

## Lingkungan Pengembangan
- Python 3.10
- Diuji pada Windows 10/11

📧 **Penulis:** Dian Pratiwi

📩 diapratiwi14044@gmail.com
