# Laporan Proyek Machine Learning - Eldy Effendi

## Project Overview

Dalam era e-commerce yang kompetitif seperti saat ini, menyediakan rekomendasi produk yang relevan kepada pelanggan adalah salah satu strategi penting untuk meningkatkan pengalaman pengguna dan nilai transaksi. Salah satu tantangan utama yang dihadapi oleh platform seperti Amazon adalah bagaimana memberikan rekomendasi yang bermakna ketika riwayat interaksi pelanggan tidak lengkap atau sangat terbatas.

Proyek ini bertujuan mengatasi dua permasalahan utama:
- **Pernyataan Masalah 1**  
  Bagaimana merekomendasikan produk yang relevan berdasarkan deskripsi dan riwayat pembelian pelanggan?

- **Pernyataan Masalah 2**  
  Bagaimana mengenali produk-produk yang sering dibeli secara bersamaan untuk kebutuhan bundling?

Untuk menjawab permasalahan ini, digunakan pendekatan *Content-Based Filtering* berbasis deskripsi produk dan frekuensi pembelian bersamaan. Meskipun pendekatannya tidak menggunakan profil lengkap pelanggan, sistem ini memanfaatkan informasi dari produk yang pernah dibeli untuk menemukan pola pembelian yang sering terjadi secara bersamaan.

Dataset yang digunakan diperoleh dari [Kaggle](https://www.kaggle.com/datasets/zahidmughal2343/amazon-sales-2025), yang berisi 250 transaksi penjualan Amazon tahun 2025. Dataset ini mencakup informasi produk, kategori, harga, kuantitas, pelanggan, lokasi, metode pembayaran, dan status pemesanan.

### Alur dan Metode Proyek

Proyek ini dibangun dengan beberapa komponen utama sebagai berikut:

1. **Fungsi `recommend_bundled_products`:**
   - Mengambil produk tertentu (misalnya "headphones").
   - Mencari pelanggan yang membeli produk tersebut.
   - Mengidentifikasi produk lain yang dibeli oleh pelanggan tersebut, selain produk yang dimaksud.
   - Menghitung frekuensi produk-produk tersebut dan menampilkan 5 produk teratas.

2. **Tabel Rekomendasi Produk:**
   - Menampilkan daftar produk yang paling sering dibeli bersamaan dengan produk target.
   - Frekuensi pembelian bersama dihitung dan disusun untuk menunjukkan hubungan antarproduk.

3. **Fungsi `evaluate_ndcg`:**
   - Mengevaluasi kualitas urutan rekomendasi menggunakan skor NDCG@5 (*Normalized Discounted Cumulative Gain*).
   - Produk relevan didefinisikan secara manual (contohnya: smartphone, book, dan laptop sebagai produk yang relevan terhadap "headphones").

4. **Proses Evaluasi NDCG:**
   - Skor NDCG@5 memperhitungkan posisi produk relevan dalam daftar rekomendasi.
   - Semakin atas posisi produk relevan, semakin tinggi bobot nilai keakuratannya.

5. **Output Akhir:**
   - Tabel produk-produk yang direkomendasikan beserta frekuensinya.
   - Skor NDCG@5 sebagai indikator seberapa baik sistem mengurutkan produk berdasarkan relevansi.
   - Dalam studi kasus ini, skor NDCG@5 adalah **0.8302**, menunjukkan bahwa sistem menghasilkan urutan rekomendasi yang cukup relevan dan berkualitas.

### Tujuan Proyek

- Mengembangkan sistem rekomendasi produk berbasis konten untuk meningkatkan pengalaman pelanggan.
- Memberikan rekomendasi produk yang sering dibeli bersama, meskipun tidak ada data pembelian lengkap dari pelanggan.
- Mengevaluasi efektivitas rekomendasi dengan metrik NDCG.

Dengan pendekatan ini, platform e-commerce dapat tetap memberikan pengalaman personalisasi yang baik walaupun data pelanggan tidak lengkap, sekaligus meningkatkan peluang penjualan silang (*cross-selling*).

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
   *Membangun sistem rekomendasi yang relevan secara konten (Content-Based).* 

- **Jawaban pernyataan masalah 2**  
   *Mengidentifikasi potensi bundling produk untuk meningkatkan nilai transaksi.*  

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah **Amazon Sales 2025** yang diambil dari [Kaggle](https://www.kaggle.com/datasets/zahidmughal2343/amazon-sales-2025). Dataset ini memuat informasi tentang transaksi penjualan Amazon sepanjang tahun 2025 dan terdiri dari **250 baris (records)** dan **11 kolom (fitur)**.

### Informasi Dataset:
- **Jumlah Data**: 250 baris dan 11 kolom
- **Kondisi Data**:
  - **Missing Value**: Tidak ditemukan nilai yang hilang pada seluruh fitur.
  - **Jumlah duplikasi ditemukan**: 0
  - **Jumlah data setelah menghapus duplikasi**: 250
  - **Outlier**:  Sebagian besar harga produk berada dalam rentang interkuartil (Q1 ke Q3), yaitu antara sekitar 50 hingga 600,
                  terlihat bahwa ada harga-harga yang jauh lebih tinggi dari rentang normal (hingga 1200), yang diklasifikasikan sebagai outlier—jumlahnya sebanyak 30 data. Deteksi ini 
                  penting agar analisis dan model tidak bias karena nilai ekstrem.
  ![download (9)](https://github.com/user-attachments/assets/73734335-b761-44cd-91be-b3b30ec3846c)  
- **Tautan Sumber Data**: [https://www.kaggle.com/datasets/zahidmughal2343/amazon-sales-2025](https://www.kaggle.com/datasets/zahidmughal2343/amazon-sales-2025)

### Uraian Fitur Dataset:

| Field               | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| **Order ID**        | ID unik untuk setiap transaksi, contohnya `ORD0001`                         |
| **Date**            | Tanggal terjadinya transaksi                                                |
| **Product**         | Nama produk yang dibeli                                                     |
| **Category**        | Kategori produk seperti Electronics, Clothing, Home Appliances, dll         |
| **Price**           | Harga satuan produk                                                         |
| **Quantity**        | Jumlah unit produk yang dibeli dalam transaksi                              |
| **Total Sales**     | Total pendapatan dari transaksi (Price × Quantity)                          |
| **Customer Name**   | Nama pelanggan yang melakukan transaksi                                     |
| **Customer Location**| Kota tempat pelanggan berdomisili                                          |
| **Payment Method**  | Metode pembayaran seperti Credit Card, Debit Card, PayPal, dll              |
| **Status**          | Status pesanan, yaitu `Completed`, `Pending`, atau `Cancelled`             |

Dataset ini bersifat tabular dan memberikan informasi menyeluruh yang cukup untuk dianalisis guna membangun sistem rekomendasi berbasis konten dengan pendekatan frekuensi produk yang sering dibeli bersamaan.

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

Tahapan pembersihan dan persiapan data dilakukan secara sistematis untuk memastikan kualitas data sebelum membangun sistem rekomendasi. Adapun langkah-langkahnya adalah sebagai berikut:

1. **Konversi Tipe Data:**
   - Kolom `Date` dikonversi ke format `datetime` agar dapat dianalisis secara temporal.

2. **Pembersihan Teks:**
   - Kolom kategorikal seperti `Product` dan `Category` dinormalisasi dengan mengubah semua huruf menjadi lowercase dan menghapus spasi di awal dan akhir (trim).
   - Kolom `Product_clean` dan `Category_clean` dibuat sebagai hasil pembersihan tersebut.

3. **Penghapusan Duplikasi:**
   - Menghapus baris duplikat pada dataset untuk memastikan tidak terjadi pengulangan informasi yang dapat mengganggu analisis.

4. **Deteksi dan Penanganan Outlier:**
   - Kolom `Price` dianalisis menggunakan boxplot untuk mendeteksi nilai outlier, meskipun tidak dilakukan penghapusan karena harga ekstrem masih dianggap relevan dalam konteks data transaksi.

5. **Pembuatan Fitur Baru:**
   - Kolom baru `description` dibuat dengan menggabungkan teks dari `Product_clean` dan `Category_clean`. Fitur ini digunakan untuk membentuk representasi semantik yang lebih kaya dari setiap produk.

6. **Ekstraksi Fitur dengan TF-IDF:**
   - Data pada kolom `description` diproses menggunakan metode **TF-IDF (Term Frequency-Inverse Document Frequency)** untuk mengubah teks menjadi representasi vektor numerik.
   - TF-IDF digunakan untuk mengukur seberapa penting sebuah kata dalam satu dokumen dibandingkan dengan dokumen lain dalam korpus.
   - Vektor TF-IDF ini kemudian menjadi dasar dalam menghitung kemiripan antar produk menggunakan metode cosine similarity, yang digunakan dalam sistem rekomendasi berbasis konten (Content-Based Filtering).

## Modeling and Results

Model sistem rekomendasi dibangun menggunakan pendekatan **Content-Based Filtering** dengan mengukur kemiripan antar produk menggunakan **Cosine Similarity**. Representasi vektor produk yang telah dihasilkan sebelumnya (dari TF-IDF) digunakan sebagai dasar perhitungan kemiripan antar produk.

Langkah-langkah utama dalam modeling:
- Membangun matriks kemiripan (similarity matrix) antar produk berdasarkan cosine similarity.
- Untuk setiap produk, sistem akan mencari dan merekomendasikan produk lain yang memiliki nilai kemiripan tertinggi.

Distribusi nilai similarity menunjukkan bahwa:
- Mayoritas pasangan produk memiliki skor kemiripan rendah, menunjukkan bahwa deskripsi produk cukup beragam.
- Sebagian kecil pasangan memiliki skor tinggi, yang mengindikasikan adanya kemiripan dalam nama atau kategori produk, atau merupakan varian dari produk yang sama.

###Contoh output:
### 🔍 Top 5 Rekomendasi Produk Mirip untuk: `Headphones`

| Produk         | Frekuensi |
|----------------|-----------:|
| smartphone     | 35         |
| smartwatch     | 34         |
| running shoes  | 27         |
| book           | 25         |
| laptop         | 24         |

📊 **Evaluasi Model - NDCG@5**
- Untuk status transaksi 'Pending', model menghasilkan skor **NDCG@5 = 0.8302**, yang menunjukkan bahwa rekomendasi yang diberikan cukup relevan terhadap data historis yang ada.


## Evaluation

Evaluasi model sistem rekomendasi ini dilakukan menggunakan metrik **NDCG@5 (Normalized Discounted Cumulative Gain)**, yang mengukur seberapa relevan produk-produk yang direkomendasikan dibandingkan dengan produk yang benar-benar relevan (dalam hal ini, berdasarkan produk yang dibeli bersama).

---

### ✨ Penjelasan Evaluasi

- Fungsi `recommend_bundled_products()` mengidentifikasi produk-produk yang paling sering dibeli bersamaan dengan `headphones` berdasarkan riwayat pembelian pelanggan. Ini menjawab *Pernyataan Masalah 2*.
- Fungsi `evaluate_ndcg()` mengukur kualitas urutan rekomendasi terhadap produk yang terbukti relevan (dalam studi kasus ini: `smartphone`, `book`, dan `laptop`).

Nilai NDCG@5 sebesar **0.8302** menunjukkan bahwa model mampu menghasilkan urutan rekomendasi yang sangat relevan terhadap konteks pembelian aktual. Nilai ini menunjukkan bahwa produk yang benar-benar relevan cenderung muncul di urutan teratas rekomendasi.

---

### 🎯 Evaluasi terhadap Problem Statement dan Goals

- **Problem Statement 1**  
  *Bagaimana merekomendasikan produk yang relevan berdasarkan deskripsi dan riwayat pembelian pelanggan?*  
  ✅ *Tercapai.* Model mampu merekomendasikan produk-produk serupa berdasarkan deskripsi (dari TF-IDF) dan memvalidasi hasilnya melalui pembelian aktual.

- **Problem Statement 2**  
  *Bagaimana mengenali produk-produk yang sering dibeli secara bersamaan untuk kebutuhan bundling?*  
  ✅ *Tercapai.* Model mengidentifikasi produk yang sering dibeli bersama menggunakan frekuensi pembelian historis antar pelanggan.

- **Goal 1**  
  *Membangun sistem rekomendasi yang relevan secara konten (Content-Based).*  
  ✅ *Tercapai.* Sistem berhasil menggunakan konten deskripsi produk untuk menghasilkan rekomendasi.

- **Goal 2**  
  *Mengidentifikasi potensi bundling produk untuk meningkatkan nilai transaksi.*  
  ✅ *Tercapai.* Rekomendasi produk bundling berhasil dihasilkan berdasarkan frekuensi pembelian bersama.

---

### 💼 Dampak terhadap Business Understanding

Dengan model ini, bisnis dapat:
- Meningkatkan peluang cross-selling melalui bundling produk yang sering dibeli bersama.
- Meningkatkan pengalaman pelanggan dengan merekomendasikan produk yang lebih relevan.
- Mengoptimalkan strategi pemasaran berdasarkan pemahaman tentang keterkaitan antar produk.

Evaluasi menunjukkan bahwa pendekatan ini tidak hanya menjawab kebutuhan teknis, tetapi juga mendukung tujuan bisnis secara langsung.
