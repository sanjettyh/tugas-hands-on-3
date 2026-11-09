# Tugas Hands-On 3: Pandas Lanjutan

**Kasus:** 03 Product Recommendation / E-Commerce Transactions (10.000 transaksi), klasifikasi biner target `status_retur` + clustering pelanggan (RFM).

## Kualitas data
| Metrik | Sebelum | Sesudah |
|---|---|---|
| n_baris | 10.000 | 9.813 |
| n_duplikat | 0 | 0 |
| n_missing | 2.177 | 520 (semua rating kosong yang disengaja) |
| n_outlier | 0 | 0 |
| label sentimen unik | 11 | 4 |

## Temuan
1. Retur 24,6% pada barang >= Rp2 juta vs 18,9% di bawahnya (62% transaksi, 88% nilai retur) -> perketat QC dan deskripsi produk di segmen ini.
2. Ulasan negatif -> 45,6% diretur vs 5,1% pada ulasan positif -> antrean CS proaktif untuk rating <= 2,4 (cek dulu urutan waktu ulasan vs retur).
3. GMV datar (Rp21,4 -> 21,6 -> 21,3 miliar); 296 pelanggan setia yang lama tak membeli memegang 14% nilai belanja -> kampanye win-back ke segmen Berisiko.

Kategori, kota, metode bayar, diskon, dan waktu tidak berkaitan nyata dengan retur (uji chi-square).

## Model tanpa leakage
Split `stratify=y` lebih dulu, lalu imputer, scaler, dan encoder (`ColumnTransformer` dalam `Pipeline`) di-fit hanya pada train. Rating dan sentimen dikeluarkan karena baru ada setelah barang diterima (AUC ≈ 0,55 tanpa vs ≈ 0,77 dengan, tanda leakage).

## Cara menjalankan
Buka `notebook.ipynb` di Google Colab, `LINK_DATASET` sudah berisi tautan Google Drive dataset (file privat; Colab akan meminta izin akun Google sekali), lalu Runtime -> Restart and run all. Jika tautan tidak bisa diakses, notebook mencari file di Google Drive, lalu meminta upload.
