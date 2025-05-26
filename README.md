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
+ Periode Data: Data smartphone dari berbagai tahun rilis
+ Format Data: 3 file CSV terpisah
+ Total Records:
  + Data smartphone: 1000+ entries
  + Rating data: 15000+ entries
  + User data: 500+ users
 
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

### Exploratory Data Analysis (EDA)

+ Distribusi Brand Smartphone

Analisis menunjukkan bahwa dataset memiliki distribusi brand yang beragam dengan dominasi beberapa brand populer seperti Samsung, Apple, dan Xiaomi.

+ Distribusi Rating Pengguna

Mayoritas pengguna memberikan rating tinggi (4-5), menunjukkan bias positif dalam penilaian atau kemungkinan hanya pengguna yang puas yang memberikan rating.

+ Aktivitas Pengguna

Distribusi aktivitas pengguna mengikuti pola long-tail, dimana sebagian kecil pengguna sangat aktif memberikan banyak rating sementara mayoritas pengguna hanya memberikan sedikit rating.

+ Spesifikasi Smartphone

  + Operating System: Dominasi Android (80%) dibandingkan iOS (20%)
  + Performance Score: Distribusi normal dengan rata-rata 6.5/10
  + Memory & RAM: Variasi yang luas dari entry-level hingga flagship
  + Camera: Range dari 8MP hingga 108MP
 
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




