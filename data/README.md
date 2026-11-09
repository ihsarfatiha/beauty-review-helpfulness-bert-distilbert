# Dataset

File dataset **tidak disertakan** di repository ini karena ukurannya melebihi
batas GitHub (maksimal 100 MB per file; dataset mentah ±204 MB).

## 1. Unduh dataset mentah

Penelitian ini memakai subset **Beauty_and_Personal_Care** dari
[Amazon Reviews 2023](https://amazon-reviews-2023.github.io/).

1. Buka halaman dataset: <https://amazon-reviews-2023.github.io/>
2. Unduh kategori **Beauty_and_Personal_Care** (format CSV).
3. Simpan file sebagai `data/Beauty_dataset_amazon.csv`.

Struktur kolom yang dipakai: `text`, `helpful_vote` (dataset asli memiliki
banyak kolom lain yang diabaikan).

## 2. Buat dataset 50K yang seimbang

Notebook sel **1** (`Metopen_final_jadi.ipynb`) melakukan:

- menghapus baris tanpa `text` / `helpful_vote`,
- memberi label `1` jika `helpful_vote > 2`, selain itu `0`,
- mengambil 25.000 sampel per kelas (total 50.000, seimbang),
- menyimpan hasil ke `data/Beauty_dataset_50k.csv`.

Jalankan notebook dari folder `data/`, atau sesuaikan path pada sel pemuatan data.

## 3. Dataset turunan

`Beauty_dataset_50k.csv` (±beberapa MB) juga di-ignore oleh Git. Jika ingin
membagikannya, gunakan Git LFS, Hugging Face Dataset, atau Google Drive, lalu
tambahkan tautannya di README.
