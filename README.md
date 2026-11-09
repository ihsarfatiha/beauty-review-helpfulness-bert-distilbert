# Analisis Prediktif Kebergunaan (Helpfulness) Ulasan E-Commerce Berbasis Model Transformer

Perbandingan **BERT vs DistilBERT** dalam memprediksi apakah sebuah ulasan
e-commerce *helpful* atau *not helpful*, dengan menekankan trade-off antara
**akurasi** dan **efisiensi komputasi**.

Proyek Metode Penelitian — Kelompok 11, Teknik Informatika, Universitas Mercu Buana.

---

## Ringkasan

Ulasan online sangat banyak, tetapi tidak semuanya informatif. Penelitian ini
memodelkan *review helpfulness* sebagai klasifikasi biner:

- `1` = Helpful → `helpful_vote > 2`
- `0` = Not Helpful → `helpful_vote <= 2`

Data diseimbangkan menjadi **50.000 ulasan** (25.000 per kelas), lalu
dibandingkan dua model Transformer:

| Model | Arsitektur | Parameter | Ukuran |
|---|---|---|---|
| **BERT** (`bert-base-uncased`) | 12 layer encoder | ±110M | ±418 MB |
| **DistilBERT** (`distilbert-base-uncased`) | 6 layer encoder | ±66M | ±255 MB |

## Hasil

### Ringkasan lintas domain (laporan)

| Domain | Model | F1-Score | Latency (ms) | Speedup |
|---|---|---:|---:|---:|
| Electronics | BERT | 0.733 | 7.88 | 1.89× |
| Electronics | **DistilBERT** | **0.738** | **4.17** | |
| Beauty | **BERT** | **0.698** | 1.39 | 1.07× |
| Beauty | DistilBERT | 0.694 | **1.30** | |
| Home | **BERT** | **0.964** | 7.75 | 1.93× |
| Home | DistilBERT | 0.962 | **4.02** | |

### Detail eksperimen domain Beauty (notebook)

| Model | F1-Score | Accuracy | Training (s) | Inference (ms/sampel) | Size (MB) | AUC |
|---|---:|---:|---:|---:|---:|---:|
| BERT | 0.698 | 0.7086 | 1190.58 | 1.399 | 417.66 | 0.7759 |
| DistilBERT | 0.694 | 0.7035 | 654.28 | 1.287 | 255.42 | 0.7736 |

**Temuan utama**

- Selisih F1-Score kedua model **di bawah 1%** di semua domain — performa praktis setara.
- DistilBERT **38–42% lebih kecil** dan hingga **1,93× lebih cepat**.
- Tidak ada model yang unggul universal; pemilihan model bergantung pada
  karakteristik bahasa tiap domain.
- DistilBERT layak menjadi model default untuk deployment real-time, sedangkan
  BERT masih relevan ketika akurasi menjadi prioritas utama.

## Struktur Repository

```
.
├── notebooks/
│   └── Metopen_final_jadi.ipynb     # EDA + training + evaluasi BERT & DistilBERT
├── docs/
│   └── Laporan_Kelompok11_Analisis_Helpfulness_BERT_DistilBERT.pdf  # Laporan (BAB I–V)
├── data/
│   └── README.md                    # Cara mengunduh & menyiapkan dataset
├── requirements.txt
├── .gitignore
└── README.md
```

> **Dataset tidak disertakan.** File mentah berukuran ±204 MB, melebihi batas
> 100 MB per file GitHub. Lihat [`data/README.md`](data/README.md).

## Menjalankan Notebook

### Opsi A — Google Colab (disarankan)

1. Buka [Colab](https://colab.research.google.com/), pilih **Upload** →
   `notebooks/Metopen_final_jadi.ipynb`.
2. Aktifkan GPU: **Runtime → Change runtime type → T4 GPU**.
3. Unggah `Beauty_dataset_amazon.csv` ke direktori kerja Colab.
4. Jalankan semua sel (**Runtime → Run all**). Notebook akan menghasilkan
   `Beauty_dataset_50k.csv`, grafik EDA, metrik, confusion matrix, dan kurva ROC.

### Opsi B — Lokal

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# Linux / macOS
source .venv/bin/activate

pip install -r requirements.txt
jupyter notebook
```

Letakkan dataset di `data/Beauty_dataset_amazon.csv`, lalu jalankan notebook dari
folder `data/` (agar path relatif pada notebook sesuai).

> Training penuh membutuhkan GPU. Tanpa GPU, waktu training akan jauh lebih lama
> (notebook otomatis memakai CPU dan mematikan mixed precision).

## Isi Notebook

1. **Load & inspeksi data** — struktur 473.823 ulasan, pembersihan kolom kosong.
2. **Pelabelan & balancing** — label `helpful_vote > 2`, sampling 25K + 25K.
3. **EDA** — word cloud per kelas, distribusi panjang ulasan, rata-rata kata.
4. **Training BERT** — 2 epoch, lr `2e-5`, batch 16, `max_len` 256.
5. **Training DistilBERT** — konfigurasi identik agar perbandingan adil.
6. **Evaluasi** — F1-Score, accuracy, training time, latency, model size,
   confusion matrix, dan ROC/AUC.

## Metrik Evaluasi

- **F1-Score** — metrik utama klasifikasi (kelas seimbang).
- **Latency** — waktu inferensi per sampel (ms).
- **Speedup** — `latency BERT / latency DistilBERT`.
- **Model size** — ukuran parameter + buffer (MB).
- **AUC** — kemampuan diskriminasi model dari kurva ROC.

## Tim

| Nama | NIM |
|---|---|
| Tegar Wijaya Kusuma | 41523010217 |
| Ali Umar Rafsanjani | 41523010191 |
| Muhammad Ihsar Fatiha | 41523010202 |

**Dosen Pengampu:** Eliyani, Dr. Ir., M.Kom.

## Referensi Utama

- Devlin et al. (2019). *BERT: Pre-training of Deep Bidirectional Transformers.*
- Sanh et al. (2019). *DistilBERT, a distilled version of BERT.*
- Mudambi & Schuff (2010). *What Makes a Helpful Online Review?*
- Hou et al. (2024). *Bridging Language and Items for Retrieval and Recommendation.*
- Amazon Reviews 2023 — <https://amazon-reviews-2023.github.io/>

## Lisensi

Proyek akademik. Kode bebas digunakan untuk keperluan pembelajaran dengan
mencantumkan sumber. Dataset mengikuti lisensi dari penyedia aslinya
(Amazon Reviews 2023).
