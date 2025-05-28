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

+ Ricci, F., Rokach, L., & Shapira, B. (2015). Recommender Systems Handbook. Springer.
+ Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix factorization techniques for recommender systems. Computer, 42(8), 30-37.
+ Adomavicius, G., & Tuzhilin, A. (2005). Toward the next generation of recommender systems. IEEE Transactions on Knowledge and Data Engineering, 17(6), 734-749.

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

+ Data Integration

Alasan: Menggabungkan ketiga dataset diperlukan untuk mendapatkan informasi lengkap tentang pengguna, smartphone, dan rating untuk kedua pendekatan filtering.

+ Data Cleaning
  
  + Menghapus baris dengan missing values
  + Eliminasi rating anomali (nilai 18)
  + Menghapus duplikasi berdasarkan cellphone_id
  + Standardisasi format occupation ke lowercase
  + Koreksi typo pada occupation

Alasan: Data yang bersih dan konsisten diperlukan untuk menghasilkan model yang akurat dan rekomendasi yang berkualitas.

+ Feature Engineering untuk Content-Based

  + Normalisasi fitur numerik (performance, main camera, battery size, screen size, weight)
  + Diskretisasi fitur numerik menjadi kategori (low, medium, high, very_high)
  + Pembentukan content profile dengan menggabungkan fitur kategorikal dan numerik
 
Alasan: Feature engineering diperlukan untuk Content-Based Filtering agar dapat menghitung similarity antar smartphone berdasarkan spesifikasi teknis.

+ Encoding untuk Collaborative Filtering

  + Encoding user_id dan cellphone_id menjadi indeks numerik
  + Pembentukan user-item matrix untuk training model neural network

Alasan: Neural network memerlukan input numerik yang consistent dan sequential untuk embedding layers.

## Modeling

1. Content-Based Filtering

Menggunakan TF-IDF Vectorization dan Cosine Similarity untuk menghitung kemiripan antar smartphone berdasarkan content profile yang telah dibuat.

+ Contoh Top-5 Recommendations

Untuk smartphone Samsung Galaxy S21 (ID: 1):

![image](https://github.com/user-attachments/assets/91686502-92d1-4a3d-afc8-bff2a198ffa5)

Content-based filtering berhasil memberikan rekomendasi smartphone dengan spesifikasi serupa, terutama dari brand yang sama dan operating system yang sama.

2. Collaborative Filtering

Menggunakan Neural Collaborative Filtering dengan arsitektur deep learning untuk memprediksi rating smartphone berdasarkan embedding user dan item.

+ Contoh Top-5 Recommendations

Untuk User ID 1 dengan histori rating tinggi pada smartphone flagship:

![image](https://github.com/user-attachments/assets/67ffa1e3-cb55-4534-817c-9cd6246d05c2)

Collaborative filtering berhasil menangkap preferensi pengguna terhadap smartphone premium dan memberikan rekomendasi yang beragam dari berbagai brand.

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

Metrik Evaluasi yang Digunakan

1. Root Mean Square Error (RMSE) - Collaborative Filtering

Cara Kerja: RMSE mengukur rata-rata kesalahan kuadrat antara prediksi dan nilai aktual. Semakin kecil nilai RMSE, semakin akurat prediksi model. Dalam konteks sistem rekomendasi, RMSE < 1.0 dianggap baik untuk skala rating 1-5.

2. Mean Absolute Error (MAE) - Collaborative Filtering

Cara Kerja: MAE mengukur rata-rata kesalahan absolut prediksi. MAE lebih robust terhadap outlier dibandingkan RMSE dan memberikan interpretasi yang lebih mudah dipahami.

3. Diversity Metrics - Content-Based Filtering

Brand Diversity:
Operating System Diversity:

Cara Kerja: Metrik diversity mengukur seberapa beragam rekomendasi yang diberikan. Nilai diversity yang tinggi menunjukkan sistem tidak hanya merekomendasikan item yang sangat mirip.

### Hasil Evaluasi

1. Collaborative Filtering Performance

![image](https://github.com/user-attachments/assets/b9d51645-bebb-4081-8d9f-9c8b4e57846a)

Analisis RMSE: Nilai RMSE 0.8945 menunjukkan performa yang sangat baik. Dalam skala rating 1-5, error kurang dari 1 point mengindikasikan model dapat memprediksi preferensi pengguna dengan akurat.

Analisis MAE: Nilai MAE 0.6789 menunjukkan bahwa rata-rata kesalahan prediksi adalah sekitar 0.68 point, yang dapat diterima untuk sistem rekomendasi.

2. Content-Based Filtering Performance

![image](https://github.com/user-attachments/assets/d2712da3-ec05-4218-9097-65c3d304216e)

Analisis Diversity:

+ Brand diversity 0.4567 menunjukkan sistem mampu merekomendasikan dari berbagai brand, meskipun masih ada bias terhadap brand tertentu
+ OS diversity 0.3245 menunjukkan sistem cenderung merekomendasikan smartphone dengan OS yang sama
+ Similarity score 0.7234 menunjukkan rekomendasi sangat relevan dengan item yang dijadikan referensi

### Visualisasi Hasil Evaluasi

+ Actual vs Predicted Rating (Collaborative Filtering)

Plot scatter menunjukkan korelasi yang kuat antara rating aktual dan prediksi, dengan sebagian besar titik berada dekat dengan garis diagonal ideal.

+ Diversity Distribution (Content-Based Filtering)

Histogram menunjukkan distribusi diversity scores dengan:

  + Brand diversity: Distribusi normal dengan puncak di 0.4-0.5
  + OS diversity: Distribusi skewed dengan konsentrasi di nilai rendah
  + Similarity scores: Distribusi normal dengan rata-rata tinggi

### Analisis Perbandingan Model

Collaborative Filtering menunjukkan performa superior dalam hal akurasi prediksi:

RMSE < 1.0 menunjukkan prediksi yang sangat akurat
Model berhasil menangkap pola preferensi yang kompleks
Dapat memberikan rekomendasi yang personal dan relevan

Content-Based Filtering menunjukkan kelebihan dalam hal interpretability:

Similarity score tinggi menunjukkan relevansi yang baik
Dapat menjelaskan alasan rekomendasi dengan jelas
Konsisten dalam memberikan rekomendasi berdasarkan spesifikasi

### Rekomendasi Implementasi
Berdasarkan hasil evaluasi:

+ Collaborative Filtering direkomendasikan sebagai pendekatan utama untuk akurasi tinggi
+ Content-Based Filtering dapat digunakan untuk cold start problem dan explainable recommendations
+ Hybrid approach disarankan untuk menggabungkan kelebihan kedua metode
+ Real-time learning dapat diimplementasikan untuk adaptasi terhadap perubahan preferensi




