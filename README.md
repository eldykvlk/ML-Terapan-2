# Laporan Proyek Machine Learning - Eldy Effendi

## Project Overview

Proyek ini berfokus pada pengembangan sistem rekomendasi produk berbasis konten (*Content-Based Filtering*) menggunakan data penjualan Amazon tahun 2025. Dataset yang digunakan diperoleh dari [Kaggle](https://www.kaggle.com/datasets/zahidmughal2343/amazon-sales-2025) dan berisi 250 transaksi penjualan, mencakup informasi seperti produk, kategori, harga, kuantitas, pelanggan, lokasi, metode pembayaran, dan status pemesanan.

Tujuan dari proyek ini adalah memberikan rekomendasi produk yang relevan kepada pelanggan berdasarkan deskripsi produk yang pernah mereka beli atau minati. Dengan pendekatan berbasis deskripsi, model ini mampu merekomendasikan produk meskipun tidak terdapat riwayat pembelian yang lengkap dari pengguna.

## Business Understanding

Amazon sebagai salah satu platform e-commerce terbesar menghadapi tantangan untuk meningkatkan kepuasan pelanggan dan nilai transaksi melalui sistem rekomendasi yang efisien. Dengan memberikan rekomendasi produk yang mirip atau sering dibeli bersama, perusahaan dapat meningkatkan peluang pembelian tambahan (*cross-selling*) dan loyalitas pelanggan.

## Problem Statements
Menjelaskan pernyataan masalah:

- **Pernyataan Masalah 1**  
  Bagaimana merekomendasikan produk yang relevan berdasarkan deskripsi dan riwayat pembelian pelanggan?

- **Pernyataan Masalah 2**  
  Bagaimana mengenali produk-produk yang sering dibeli secara bersamaan untuk kebutuhan bundling?

## Goals
Menjelaskan tujuan proyek yang menjawab pernyataan masalah:

- **Jawaban pernyataan masalah 1**  
  Membangun sistem rekomendasi berbasis *Content-Based Filtering* menggunakan teknik TF-IDF dan Cosine Similarity untuk memberikan rekomendasi produk yang mirip berdasarkan deskripsi produk dan kategorinya.

- **Jawaban pernyataan masalah 2**  
  Mengembangkan fungsi rekomendasi bundling dengan menganalisis data pembelian historis pelanggan, untuk mengidentifikasi produk yang sering dibeli bersamaan oleh pelanggan yang sama.

## Data Understanding

| Field             | Description                             |
|------------------|-----------------------------------------|
| **Order ID**      | ID unik transaksi                       |
| **Date**          | Tanggal transaksi                       |
| **Product**       | Nama produk                             |
| **Category**      | Kategori produk                         |
| **Price**         | Harga per unit                          |
| **Quantity**      | Jumlah pembelian                        |
| **Total Sales**   | Total pendapatan dari transaksi         |
| **Customer Name** | Nama pelanggan                          |
| **Customer Location** | Kota pelanggan                    |
| **Payment Method**| Metode pembayaran                       |

![download (5)](https://github.com/user-attachments/assets/fe6bfee2-f0a6-415e-a67a-f5ffc5839b20)
![download (6)](https://github.com/user-attachments/assets/9c243906-3b1e-492f-8653-37dac67aa911)
![download (7)](https://github.com/user-attachments/assets/5686bcb0-0075-4606-b6ae-85b6f2728934)

Analisis awal menunjukkan:
- Tidak terdapat data hilang.
- Sebagian besar produk dijual dengan harga di bawah 200 dan kuantitas 1–5 unit.
- Kategori terpopuler: *Electronics* dan *Footwear*.
- Lokasi pelanggan yang dominan: *New York* dan *San Francisco*.
- Korelasi tinggi antara `Price`, `Quantity`, dan `Total Sales`.

## Data Preparation

Tahapan pembersihan dan persiapan data meliputi:
- Konversi kolom `Date` ke format `datetime`.
- Normalisasi teks pada kolom kategorikal (lowercase dan trim).
- Penghapusan duplikasi.
- Deteksi dan analisis outlier pada kolom `Price`.
- Pembuatan fitur baru `description` dengan menggabungkan `Product_clean` dan `Category_clean`.

## Modeling

Model rekomendasi dibangun menggunakan:
- **TF-IDF Vectorization** untuk mengubah deskripsi produk menjadi representasi numerik.
- **Cosine Similarity** untuk menghitung kemiripan antar produk.

Distribusi similarity menunjukkan bahwa:
- Mayoritas pasangan produk memiliki similarity rendah, mencerminkan deskripsi yang berbeda.
- Sebagian kecil memiliki similarity tinggi, mencerminkan produk yang mirip atau varian dari produk yang sama.

## Evaluation

![download (8)](https://github.com/user-attachments/assets/4802ab17-3f17-41e7-ab9f-0809215a911b)
Evaluasi dilakukan dengan:
- Menampilkan histogram distribusi nilai cosine similarity.
- Analisis semantik menunjukkan bahwa pasangan dengan nilai similarity tinggi memiliki deskripsi yang serupa dan relevan untuk direkomendasikan.
