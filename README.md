# Laporan Proyek Sistem Rekomendasi Smartphone

Proyek Machine Learning untuk mengembangkan sistem rekomendasi smartphone menggunakan pendekatan Content-Based Filtering dan Collaborative Filtering.

## Project Overview

### Latar Belakang

Dalam era digital saat ini, pasar smartphone berkembang sangat pesat dengan ribuan model yang tersedia dari berbagai brand. Konsumen sering mengalami kesulitan dalam memilih smartphone yang sesuai dengan kebutuhan dan preferensi mereka karena banyaknya pilihan yang tersedia. Fenomena "choice overload" ini menciptakan kebutuhan akan sistem rekomendasi yang dapat membantu konsumen menemukan smartphone yang tepat.

E-commerce dan platform review teknologi membutuhkan sistem rekomendasi yang efektif untuk meningkatkan kepuasan pelanggan dan conversion rate. Menurut penelitian dari MIT Sloan, sistem rekomendasi yang baik dapat meningkatkan penjualan hingga 35% dan engagement pengguna hingga 60%.

### Mengapa Proyek Ini Penting?

+ Membantu pengguna menemukan smartphone yang sesuai dengan preferensi tanpa harus melalui ratusan pilihan
+ Mengurangi waktu yang dibutuhkan konsumen untuk research dan membandingkan spesifikasi
+ Meningkatkan conversion rate dan customer satisfaction pada platform e-commerce
+ Memberikan rekomendasi yang disesuaikan dengan riwayat rating dan preferensi individual pengguna

### Referensi

+ Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender Systems Handbook. Springer. https://www.researchgate.net/publication/227268858_Recommender_Systems_Handbook
+ Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix factorization techniques for recommender systems. Computer, 42(8), 30-37.
https://ieeexplore.ieee.org/document/5197422

## Business Understanding

### Problem Statements

Berdasarkan analisis kebutuhan bisnis dan pengalaman pengguna, permasalahan utama yang ingin diselesaikan adalah:

+ Konsumen kesulitan memilih smartphone yang tepat dari ratusan pilihan yang tersedia di pasar
+ Platform e-commerce belum memberikan rekomendasi yang dipersonalisasi berdasarkan preferensi individual pengguna
+ Terlalu banyak informasi spesifikasi teknis yang membingungkan konsumen awam
+ Tingkat interaksi pengguna dengan platform rendah karena sulitnya menemukan produk yang relevan

### Goals

Tujuan dari proyek sistem rekomendasi smartphone ini adalah:

+ Mengembangkan sistem rekomendasi hybrid yang menggabungkan Content-Based Filtering dan Collaborative Filtering
+ Memberikan rekomendasi personal berdasarkan spesifikasi smartphone dan pola rating pengguna
+ Meningkatkan akurasi prediksi dengan target RMSE < 1.0 untuk Collaborative Filtering
+ Mencapai diversity score minimal 0.5 untuk memastikan variasi rekomendasi yang cukup
+ Menghasilkan top-N recommendations yang relevan dan mudah dipahami pengguna

### Solution Approach

1. Content-Based Filtering

Merekomendasikan smartphone berdasarkan kesamaan spesifikasi teknis dengan smartphone yang disukai pengguna sebelumnya.

Fitur yang digunakan:

+ Brand smartphone
+ Operating System (Android/iOS)
+ Performance score
+ Kamera utama (megapixel)
+ Kapasitas baterai
+ Ukuran layar
+ Berat smartphone

Kelebihan:

+ Tidak memerlukan data pengguna lain (mengatasi cold start problem)
+ Dapat menjelaskan alasan rekomendasi dengan jelas
+ Bekerja baik untuk pengguna dengan preferensi yang konsisten
+ Tidak terpengaruh oleh bias popularitas

Kekurangan:

+ Terbatas pada fitur yang tersedia dalam metadata
+ Cenderung memberikan rekomendasi yang homogen
+ Sulit menemukan smartphone yang unexpected namun relevan
+ Memerlukan feature engineering yang baik

2. Collaborative Filtering

Menggunakan Neural Network dengan embedding layers untuk memprediksi rating smartphone berdasarkan pola preferensi pengguna dan item similarity.

Arsitektur Model:

+ User dan Item Embedding layers (50 dimensi)
+ Dense layers dengan dropout untuk regularization
+ Output layer untuk prediksi rating (1-5)

Kelebihan:

+ Dapat menangkap pola kompleks dalam preferensi pengguna
+ Mampu memberikan rekomendasi serendipitous
+ Memanfaatkan collective intelligence dari semua pengguna
+ Tidak memerlukan feature engineering yang kompleks

Kekurangan:

+ Cold start problem untuk pengguna atau smartphone baru
+ Memerlukan data rating yang cukup banyak
+ Model kompleks yang membutuhkan computational resources tinggi
+ Sulit untuk diinterpretasi

## Data Understanding

### Informasi Dataset

+ Sumber Data: Dataset Smartphone dari Kaggle (cellphones.zip)
+ Format Data: 3 file CSV terpisah
  + Data smartphone (data.csv):  33 data ponsel, 14 atribut
  + Rating data (rating.csv): 990 data penilaian, 3 kolom (user_id, cellphone_id, rating)
  + User data (user.csv): 99 pengguna, 4 kolom (user_id, age, gender, occupation).
 
### Kondisi Data

Setelah melakukan pemeriksaan awal, kondisi dataset adalah sebagai berikut:

+ Missing Values: Ditemukan beberapa baris dengan nilai kosong yang perlu dibersihkan
+ Duplicate Records: Ada duplikasi pada cellphone_id yang perlu dihapus
+ Anomali Data: Rating dengan nilai 18 (outlier) yang perlu difilter
+ Data Quality Issues: Typo pada kolom occupation ('healthare' → 'healthcare', 'it' → 'information technology')

### Variabel/Fitur dalam Dataset

+ cellphone data.csv/data.csv

![image](https://github.com/user-attachments/assets/992fe606-2153-42f9-bc07-2985c5570897)

+ cellphone ratings.csv/rating.csv

![image](https://github.com/user-attachments/assets/8df6a6c4-8061-42ca-8392-fbf852443f5b)

+ cellphone users.csv/user.csv

![image](https://github.com/user-attachments/assets/ba7f6225-3e9f-4d13-a555-9f0a1930b583)

## Exploratory Data Analysis (EDA)

1. Data

+ Distribusi Brand Smartphone

![image](https://github.com/user-attachments/assets/a98a353e-d30a-4c0e-8e50-8511012ca071)

Analisis menunjukkan bahwa dataset memiliki distribusi brand yang beragam dengan di dominasi oleh Samsung dengan 8 model, diikuti Apple dengan 6, sementara merek lain memiliki jumlah model yang lebih sedikit dan bervariasi.

+ Distribusi Sistem Operasi

![image](https://github.com/user-attachments/assets/9e8cc576-9379-4d46-b861-a40b941d19a4)

Distribusi sistem operasi didominasi oleh Android dengan 27 model, sementara iOS hanya memiliki 6 model dalam dataset.

+ Distribusi Kapasitas Memori Internal

![image](https://github.com/user-attachments/assets/8eb3e507-47c9-474a-bcee-a745e70d983f)

Kapasitas memori internal yang paling umum adalah 128GB dengan 20 model, diikuti oleh 256GB, 64GB, 32GB, dan hanya satu model dengan 512GB.

+ Distribusi Kapasitas RAM

![image](https://github.com/user-attachments/assets/b3986cc5-221d-44bc-97df-9c74ca807d23)

Kapasitas RAM paling banyak tersedia adalah 8GB dengan 13 model, diikuti oleh 4GB dan 6GB masing-masing 6 model, serta 3GB dan 12GB yang lebih sedikit.

+  Kategorisasi Performa

![image](https://github.com/user-attachments/assets/e8c8ad13-7e45-4e4f-9dda-2c8a549dc6f8)

Terdapat 23 smartphone dengan performa tinggi (skor > 5) dan 10 smartphone dengan performa rendah (skor ≤ 5) dalam dataset.

+ Analisis Kamera Utama

![image](https://github.com/user-attachments/assets/d31ab2f4-15ba-4c5d-83b1-385f02c47647)

Resolusi kamera utama yang paling umum adalah 50MP dengan 13 model, diikuti 12MP dengan 10 model, serta beberapa model dengan resolusi lebih tinggi seperti 64MP, 48MP, dan 108MP.

+ Analisis Kapasitas Baterai

![image](https://github.com/user-attachments/assets/ec6e3c60-731f-4bc4-9d77-96956380c33e)

Kapasitas baterai 5000mAh paling umum dengan 11 model, sementara kapasitas lainnya bervariasi dari 2018mAh hingga 5003mAh dengan jumlah model lebih sedikit.

+ Analisis Ukuran Layar

![image](https://github.com/user-attachments/assets/b2720a1d-69ed-490c-bba0-899dfd2b9d44)

Ukuran layar 6.7 inci paling umum dengan 8 model, diikuti ukuran 6.5 inci dan 6.1 inci, serta variasi ukuran layar lain yang lebih kecil jumlahnya.

+ Analisis Berat Smartphone

![image](https://github.com/user-attachments/assets/34779463-8e9d-444e-b512-71a0e8779a17)

Berat smartphone bervariasi dalam 27 kategori berbeda, dengan berat 204 gram paling sering muncul sebanyak 5 model.

+ Analisis Tahun Rilis

![image](https://github.com/user-attachments/assets/79f59dc3-3500-449b-9bee-df59c52972d4)

Smartphone dalam dataset dirilis terutama pada tahun 2021 dan 2022 dengan jumlah yang sama, sementara hanya satu model yang dirilis pada 2018.

2. Rating

+ Aktivitas Penguna

![image](https://github.com/user-attachments/assets/3babe50d-c838-4347-bcdb-97de260f0ebb)

Setiap user dalam dataset rating memberikan tepat 10 ulasan, menunjukkan distribusi aktivitas review yang seragam antar pengguna.

+ Popularitas Smartphone

![image](https://github.com/user-attachments/assets/086bfd55-5f29-4a67-81c7-65866b457831)

Jumlah review per smartphone berkisar antara 20 hingga 41, menunjukkan variasi popularitas antar model.

+ Distribusi Rating

![image](https://github.com/user-attachments/assets/2030b384-cc0e-4376-9d34-d57721caba39)

Sebagian besar pengguna memberikan rating tinggi (7–10), dengan rating 8 sebagai yang paling sering muncul.

3. User

+ Distribusi Rating

![image](https://github.com/user-attachments/assets/b30f2178-5caa-44e0-ba58-e41d11a7489a)

Distribusi usia pengguna beragam, dengan konsentrasi tertinggi pada usia 25 tahun dan rentang usia pengguna dari 21 hingga 61 tahun.

+ Distribusi Gender

![image](https://github.com/user-attachments/assets/f5a75de9-f3d2-4ee9-bbf1-8fc36f0ed814)

Mayoritas pengguna adalah laki-laki (50) dan perempuan (46), dengan (3) data yang belum terisi pada kategori gender.

+ Distribusi Pekerjaan

Pengguna memiliki beragam pekerjaan dengan 45 variasi berbeda, paling banyak berprofesi sebagai manager, information technology, dan IT.

## Data Preparation

1. Integrasi Data

**Proses Penggabungan Dataset**
Preprocessing menggabungkan tiga dataset utama:

1. **Dataset Rating** (`rating_df`) - Berisi rating pengguna untuk smartphone
   - Menyimpan hubungan antara user_id dan cellphone_id
   - Mencatat nilai rating yang diberikan pengguna

2. **Dataset Smartphone** (`data_df`) - Berisi spesifikasi dan detail smartphone
   - Informasi teknis seperti brand, model, operating system
   - Spesifikasi hardware dan fitur smartphone

3. **Dataset User** (`user_df`) - Berisi informasi demografis pengguna
   - Data profil pengguna seperti gender, occupation, age
   - Informasi preferensi dan karakteristik pengguna

**Langkah Penggabungan:**
```python
# Langkah 1: Menggabungkan rating dengan data smartphone
combined_ratings = rating_df.merge(data_df, on='cellphone_id', how='inner')

# Langkah 2: Menggabungkan dengan data pengguna
final_dataset = combined_ratings.merge(user_df, on='user_id', how='inner')
```

**Hasil:** Dataset terintegrasi memberikan informasi lengkap yang menghubungkan spesifikasi smartphone, profil pengguna, dan rating yang diberikan.

2. Penilaian Kualitas Data

**Analisis Missing Value**
- **Kondisi Awal:** Hanya kolom `occupation` yang memiliki missing value (10 data kosong)
- **Sumber Masalah:** Semua missing value berasal dari `user_id 53`
- **Masalah Tambahan:** User yang sama memiliki entry gender tidak valid ("-Select Gender-")

**Penjelasan:** Missing value pada occupation menunjukkan data tidak lengkap dari satu pengguna tertentu, yang dapat mempengaruhi kualitas analisis demografis.

**Operasi Pembersihan Data**

+ Penanganan Missing Value
```python
# Menghapus baris dengan missing values
final_dataset.dropna(inplace=True)
```
**Hasil:** Eliminasi lengkap missing value di seluruh kolom.

**Penjelasan:** Penghapusan baris dilakukan karena jumlah missing value relatif kecil (10 dari total data) dan berasal dari satu user yang datanya tidak konsisten.

+ Penghapusan Anomali
```python
# Menghapus nilai rating anomali
final_dataset = final_dataset.loc[final_dataset['rating'] != 18]
```
**Alasan:** Nilai rating 18 diidentifikasi sebagai outlier yang tidak konsisten dengan skala rating yang diharapkan.

**Penjelasan:** Rating 18 kemungkinan adalah kesalahan input atau data corrupt yang dapat mengganggu akurasi model rekomendasi.

+ Standardisasi Data
**Normalisasi Field Occupation:**
- Konversi semua entry ke lowercase untuk konsistensi
- Perbaikan typo: 'healthare' → 'healthcare'
- Perluasan singkatan: 'it' → 'information technology'

```python
final_dataset['occupation'] = final_dataset['occupation'].str.lower()
final_dataset['occupation'] = final_dataset['occupation'].str.replace('healthare', 'healthcare')
final_dataset['occupation'] = final_dataset['occupation'].str.replace('it', 'information technology')
```

**Penjelasan:** Standardisasi occupation penting untuk memastikan kategorisasi yang konsisten dalam analisis demografis dan clustering pengguna.

3. Persiapan Fitur

**Penghapusan Duplikasi**
```python
# Menghapus duplikat berdasarkan ID smartphone
processed_data.drop_duplicates(subset=['cellphone_id'], inplace=True)
```

**Penjelasan:** Duplikasi dapat terjadi karena satu smartphone dinilai oleh banyak pengguna. Penghapusan duplikat diperlukan untuk analisis karakteristik smartphone yang unik.

**Ekstraksi Fitur**
Fitur-fitur penting diekstraksi dan dikonversi ke format list:

- `cellphone_id`: Identifikator unik smartphone
  - Digunakan sebagai primary key untuk referensi smartphone
  
- `brand`: Manufaktur smartphone  
  - Penting untuk analisis preferensi brand dan clustering
  
- `model`: Model smartphone spesifik
  - Memberikan granularitas tinggi untuk rekomendasi
  
- `operating_system`: Sistem operasi mobile
  - Faktor penting dalam preferensi pengguna

**Dimensi Dataset Akhir:** 33 smartphone unik setelah deduplication.

**Penjelasan:** Pengurangan menjadi 33 smartphone unik memungkinkan fokus pada karakteristik distinct setiap produk tanpa redundansi.

**Struktur Dataset Akhir**
```python
phone_dataset = pd.DataFrame({
    'cellphone_id': id_list,
    'brand': brand_list,
    'model': model_list,
    'operating_system': os_list,
})
```

**Penjelasan:** DataFrame final berisi fitur-fitur core yang cukup untuk membangun sistem rekomendasi berbasis content-based filtering.

4. Hasil Utama

**Peningkatan Kualitas Data:**
- Nol missing value - Data lengkap untuk semua fitur
- Penghapusan rating anomali - Konsistensi skala rating
- Standardisasi entry occupation - Format data seragam
- Eliminasi smartphone duplikat - Keunikan data produk
- Format data konsisten - Siap untuk pemodelan

**Dataset Bersih Final:**
- **Jumlah Record:** 33 smartphone unik
- **Fitur:** 4 atribut core (ID, brand, model, OS)
- **Kualitas:** Sepenuhnya dibersihkan dan distandarisasi
- **Kesiapan:** Siap untuk pemodelan sistem rekomendasi

5. Penggunaan
Dataset `phone_dataset` yang telah diproses siap untuk:

- **Content-based Filtering:** Algoritma rekomendasi berdasarkan similarity fitur
- **Analisis Similarity:** Perbandingan karakteristik antar smartphone
- **Training Model:** Pelatihan model machine learning untuk rekomendasi
- **Feature Engineering:** Pengembangan fitur lanjutan untuk model yang lebih kompleks

Dataset ini optimal untuk sistem rekomendasi skala kecil hingga menengah dengan focus pada karakteristik teknis smartphone.

## Modeling

1. Content-Based Filtering

Menggunakan TF-IDF Vectorization dan Cosine Similarity untuk menghitung kemiripan antar smartphone berdasarkan content profile yang telah dibuat.

![image](https://github.com/user-attachments/assets/fff837f2-e5b6-4477-ab02-a6b912d05174)

Content-based similarity matrix sudah terbentuk dengan 33 smartphone dan matriks similarity berdimensi (33, 33) yang sesuai.

+ Contoh Top-5 Recommendations

![image](https://github.com/user-attachments/assets/be8ecce6-621f-4b9c-a1cf-e45d93044d2c)

Rekomendasi menunjukkan bahwa smartphone dengan fitur dan merek serupa (Motorola Moto G series) memiliki kesamaan konten sangat tinggi, sementara perangkat dari merek lain seperti Samsung meskipun masih Android, memiliki kesamaan yang jauh lebih rendah.

2. Collaborative Filtering

Menggunakan Neural Collaborative Filtering dengan arsitektur deep learning untuk memprediksi rating smartphone berdasarkan embedding user dan item.

+ Contoh Top-5 Recommendations

![image](https://github.com/user-attachments/assets/f1cacd85-2ccc-4e58-b1db-724d8f54335c)

User ID 0 menunjukkan preferensi kuat terhadap Samsung Galaxy S22 (rating 9), yang dapat digunakan sebagai acuan utama dalam merekomendasikan smartphone sejenis dengan performa tinggi dan sistem operasi Android.

### Perbandingan Kedua Pendeketan

1. Content-Based Filtering

+ Kelebihan:

  + Memberikan rekomendasi berdasarkan spesifikasi teknis yang jelas
  + Tidak memerlukan data pengguna lain
  + Dapat menjelaskan alasan rekomendasi
  + Konsisten dengan preferensi pengguna

+ Kekurangan:

  + Rekomendasi cenderung homogen (brand dan OS yang sama)
  + Tidak dapat menangkap preferensi yang kompleks
  + Terbatas pada fitur yang tersedia

2. Collaborative Filtering

+ Kelebihan:

  + Dapat memberikan rekomendasi yang beragam dan mengejutkan
  + Menangkap pola preferensi yang kompleks
  + Memanfaatkan wisdom of crowds
  + Adaptif terhadap perubahan preferensi

+ Kekurangan:

  + Cold start problem untuk pengguna baru
  + Memerlukan data rating yang banyak
  + Sulit untuk diinterpretasi
  + Computational intensive

## Evaluation

###Metrik Evaluasi yang Digunakan

1. Root Mean Squared Error (RMSE) - Collaborative Filtering

Formula:

`RMSE = √(Σ(yi - ŷi)² / n)`

Cara Kerja:

Mengukur rata-rata kuadrat selisih antara rating aktual dan prediksi
Memberikan penalti lebih besar pada error yang lebih besar
Semakin rendah nilai RMSE, semakin baik performa model

2. Mean Absolute Error (MAE) - Collaborative Filtering
   
Formula:

`MAE = Σ|yi - ŷi| / n`

Cara Kerja:

Mengukur rata-rata absolut selisih antara rating aktual dan prediksi
Lebih robust terhadap outlier dibanding RMSE
Interpretasi langsung dalam skala rating (1-5)

3. Brand Diversity - Content-Based Filtering
   
Formula:

`Brand Diversity = Unique Brands / Total Recommendations`

Cara Kerja:

Mengukur keberagaman merek dalam rekomendasi
Nilai 1.0 berarti semua rekomendasi dari merek berbeda
Nilai rendah menunjukkan kurangnya variasi

4. OS Diversity - Content-Based Filtering
   
Formula:

`OS Diversity = Unique OS / Total Recommendations`

Cara Kerja:

Mengukur keberagaman sistem operasi dalam rekomendasi
Mencegah bias terhadap satu jenis OS
Nilai tinggi menunjukkan rekomendasi yang lebih seimbang

5. Average Similarity Score - Content-Based Filtering
   
Formula:

`Avg Similarity = Σ(similarity_scores) / n`

Cara Kerja:

Mengukur rata-rata kemiripan konten dalam rekomendasi
Nilai tinggi menunjukkan relevansi konten yang baik
Perlu diimbangi dengan diversity untuk menghindari redundansi

### Hasil Evaluasi

1. Collaborative Filtering

RMSE: 3.1312 - Error prediksi rata-rata ~3.13 poin dalam skala 1-5
MAE: 2.4619 - Simpangan rata-rata ~2.46 poin dari rating aktual
Interpretasi: Performa moderat, masih ada ruang untuk perbaikan accuracy

2. Content-Based Filtering

Brand Diversity: 0.3200 - 32% rekomendasi dari merek yang berbeda
OS Diversity: 0.2800 - 28% rekomendasi dari OS yang berbeda
Average Similarity: 0.4721 - Tingkat kemiripan konten yang memadai

Interpretasi: Sistem menghasilkan rekomendasi relevan namun kurang beragam, cenderung fokus pada smartphone serupa.

### Kesimpulan Evaluasi

Collaborative Filtering menunjukkan kemampuan prediksi yang dapat diterima namun memerlukan perbaikan untuk meningkatkan akurasi
Content-Based Filtering efektif dalam menemukan smartphone serupa tetapi perlu peningkatan diversity
Kombinasi kedua pendekatan (hybrid approach) dapat mengoptimalkan kelebihan masing-masing sistem



