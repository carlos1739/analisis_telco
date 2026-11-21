# Milestone 2: Eksperimen Model Churn Telco

Dokumen ini menjelaskan eksperimen lanjutan pada [`notebook/milestone2_churn.ipynb`](notebook/milestone2_churn.ipynb). Untuk latar belakang proyek, pembersihan data, EDA, dan hasil awal, lihat [README Milestone 1](README_Milestone1.md) dan [`notebook/milestone1_churn.ipynb`](notebook/milestone1_churn.ipynb).

## Tujuan

Membandingkan model tambahan dengan baseline dan kandidat Milestone 1 menggunakan data, preprocessing, pembagian fold, dan metrik yang konsisten. Tujuannya adalah menghasilkan perbandingan eksperimen yang dapat ditelusuri, bukan menetapkan model siap produksi.

## Protokol evaluasi

| Komponen | Pengaturan |
|---|---|
| Data | Telco Customer Churn, 7.043 pelanggan |
| Fitur | Fitur Milestone 1 ditambah `TenureGroup` dan `TotalServices` |
| Preprocessing | `StandardScaler` untuk numerik dan `OneHotEncoder` untuk kategorikal di dalam `Pipeline` |
| Validasi | `RepeatedStratifiedKFold`, 5 fold × 3 pengulangan, `random_state=42` |
| Metrik | Accuracy, Precision, Recall, F1, dan PR-AUC |
| Tuning model tambahan | Opsional; jika digunakan, `GridSearchCV` 3-fold dijalankan di dalam setiap fold luar dan memakai `average_precision` |

Model Milestone 1—baseline mayoritas, Logistic Regression, KNN, Decision Tree, Random Forest, dan HistGradientBoosting—dijalankan ulang sebagai pembanding. Sel reproduksi memeriksa apakah rata-rata metriknya masih dalam toleransi 0,005 dari hasil acuan Milestone 1.

## Model tambahan dan status notebook

Notebook menyediakan dua bagian untuk mengisi model tambahan:

- **M2.2:** model tambahan 1.
- **M2.3:** model tambahan 2.

Pada notebook saat ini, `varian_model_1` dan `varian_model_2` masih berupa dictionary kosong. Artinya, notebook belum menjalankan model tambahan dan tabel yang dihasilkan hanya membandingkan kandidat Milestone 1 sampai kedua bagian tersebut diisi dan notebook dijalankan ulang.

Jika melakukan tuning, parameter yang dipilih dicatat per fold luar. Untuk model yang mendukung `class_weight`, notebook menyarankan membandingkan varian dengan dan tanpa pembobotan kelas. Perubahan model dan alasannya sebaiknya dicatat pada laporan.

## Menjalankan notebook

Ikuti petunjuk instalasi dan penyiapan Python pada [README Milestone 1](README_Milestone1.md#12-menjalankan-notebook), kemudian dari root repositori jalankan:

```powershell
jupyter lab
```

Buka `notebook/milestone2_churn.ipynb` dan jalankan seluruh sel dari atas ke bawah. Notebook ini dapat memakai variabel `telco` jika sel-selnya dijalankan dalam kernel yang sama setelah Milestone 1, misalnya dengan menyalinnya ke notebook Milestone 1. Jika dijalankan sebagai notebook terpisah, notebook akan memuat dan membersihkan dataset sendiri bila file lokal belum tersedia; dibutuhkan koneksi internet.

## Hasil dan artefak

Notebook membuat artefak berikut di folder `results/`:

- `perbandingan_model_milestone2.csv` — tabel metrik rata-rata dan standar deviasi tiap model.
- `konfigurasi_eksperimen.json` — protokol evaluasi, konfigurasi model, dan parameter tuning terpilih.

Folder dan file tersebut dibuat saat notebook dijalankan; hasil yang dihasilkan bisa berbeda jika konfigurasi atau lingkungan pustaka berubah. Periksa artefak setelah eksekusi sebelum mengutip angka pada laporan.

## Catatan interpretasi

- Kandidat Milestone 1 dijalankan tanpa tuning, sedangkan tuning model tambahan—jika diaktifkan—menggunakan validasi bersarang. Perbedaan ini perlu disebut saat membandingkan hasil.
- PR-AUC menjadi metrik utama karena kelas churn lebih sedikit. Recall dan Precision melengkapi penilaian pada threshold 0,5.
- Fold pada repeated cross-validation saling berkaitan. Standar deviasi antar-fold bukan interval kepercayaan, dan selisih kecil tidak cukup untuk menyatakan satu model unggul.
- Laporkan hasil apa adanya, termasuk jika model tambahan tidak mengungguli baseline atau kandidat sebelumnya. Evaluasi ini belum membuktikan generalisasi ke periode baru atau kesiapan operasional.
