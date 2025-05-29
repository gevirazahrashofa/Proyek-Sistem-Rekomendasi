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

+ Fitur yang digunakan:

    + Brand smartphone
    + Operating System (Android/iOS)
    + Performance score
    + Kamera utama (megapixel)
    + Kapasitas baterai
    + Ukuran layar
    + Berat smartphone

+ Kelebihan:

    + Tidak memerlukan data pengguna lain (mengatasi cold start problem)
    + Dapat menjelaskan alasan rekomendasi dengan jelas
    + Bekerja baik untuk pengguna dengan preferensi yang konsisten
    + Tidak terpengaruh oleh bias popularitas

+ Kekurangan:

    + Terbatas pada fitur yang tersedia dalam metadata
    + Cenderung memberikan rekomendasi yang homogen
    + Sulit menemukan smartphone yang unexpected namun relevan
    + Memerlukan feature engineering yang baik

2. Collaborative Filtering

Menggunakan Neural Network dengan embedding layers untuk memprediksi rating smartphone berdasarkan pola preferensi pengguna dan item similarity.

+ Arsitektur Model:

    + User dan Item Embedding layers (50 dimensi)
    + Dense layers dengan dropout untuk regularization
    + Output layer untuk prediksi rating (1-5)

+ Kelebihan:

    + Dapat menangkap pola kompleks dalam preferensi pengguna
    + Mampu memberikan rekomendasi serendipitous
    + Memanfaatkan collective intelligence dari semua pengguna
    + Tidak memerlukan feature engineering yang kompleks

+ Kekurangan:

    + Cold start problem untuk pengguna atau smartphone baru
    + Memerlukan data rating yang cukup banyak
    + Model kompleks yang membutuhkan computational resources tinggi
    + Sulit untuk diinterpretasi

## Data Understanding

### Informasi Dataset

+ Sumber Data: Dataset Smartphone dari Kaggle https://www.kaggle.com/datasets/meirnizri/cellphones-recommendations/data
+ Format Data: 3 file CSV terpisah
  + Data smartphone (cellphones data/data.csv):  33 data ponsel, 14 atribut
  + Rating data (cellphones ratings/rating.csv): 990 data penilaian, 3 kolom (user_id, cellphone_id, rating)
  + User data (cellphones users/user.csv): 99 pengguna, 4 kolom (user_id, age, gender, occupation).

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

Proses Penggabungan Dataset

Preprocessing menggabungkan tiga dataset utama:

+ Dataset Rating (rating_df) - Berisi rating pengguna untuk smartphone
  + Menyimpan hubungan antara user_id dan cellphone_id
  + Mencatat nilai rating yang diberikan pengguna
+ Dataset Smartphone (data_df) - Berisi spesifikasi dan detail smartphone
  + Informasi teknis seperti brand, model, operating system
  + Spesifikasi hardware dan fitur smartphone
+ Dataset User (user_df) - Berisi informasi demografis pengguna
  + Data profil pengguna seperti gender, occupation, age
  + Informasi preferensi dan karakteristik pengguna
 
![image](https://github.com/user-attachments/assets/0c95b032-8815-44a6-a801-82e59489e2ae)

Hasil: Dataset terintegrasi memberikan informasi lengkap yang menghubungkan spesifikasi smartphone, profil pengguna, dan rating yang diberikan.

2. Penilaian Kualitas Data

+ Analisis Missing Value

  + Kondisi Awal: Hanya kolom occupation yang memiliki missing value (10 data kosong)
  + Sumber Masalah: Semua missing value berasal dari user_id 53
  + Masalah Tambahan: User yang sama memiliki entry gender tidak valid ("-Select Gender-")

Penjelasan: Missing value pada occupation menunjukkan data tidak lengkap dari satu pengguna tertentu, yang dapat mempengaruhi kualitas analisis demografis.

+ Operasi Pembersihan Data
  + Penanganan Missing Value
  
    ![image](https://github.com/user-attachments/assets/72d4e458-9a0b-47c3-861c-1f0dee4d20ce)

  Hasil: Eliminasi lengkap missing value di seluruh kolom.
  
  Penjelasan: Penghapusan baris dilakukan karena jumlah missing value relatif kecil (10 dari total data) dan berasal dari satu user yang datanya tidak konsisten, sehingga penghapusan tidak signifikan mengurangi volume data yang berguna.

  + Penghapusan Anomali
    
    ![image](https://github.com/user-attachments/assets/9b071624-1815-4f8c-8d39-ac818b81f8e0)
    
    Alasan: Nilai rating 18 diidentifikasi sebagai outlier yang tidak konsisten dengan skala rating yang diharapkan (kemungkinan kesalahan input atau data corrupt).
    
    Penjelasan: Rating 18 kemungkinan adalah kesalahan input atau data corrupt yang dapat mengganggu akurasi model rekomendasi karena berada di luar rentang rating yang valid (1-10) yang diamati dalam dataset.

  + Standardisasi Data
 
    ![image](https://github.com/user-attachments/assets/f23c2511-8b86-4c52-91f3-0cc32a540985)

    Normalisasi Field Occupation:
    
    + Konversi semua entry ke lowercase untuk konsistensi
    + Perbaikan typo: 'healthare' → 'healthcare'
    + Perluasan singkatan: 'it' → 'information technology'
      
    Penjelasan: Standardisasi occupation penting untuk memastikan kategorisasi yang konsisten dalam analisis demografis dan clustering pengguna, serta untuk meminimalisir variasi data yang tidak perlu akibat kesalahan penulisan.

3. Persiapan Fitur per Pendekatan

+ Content-Based Filtering Data Preparation
  
Untuk pendekatan Content-Based Filtering, data dipersiapkan dengan fokus pada fitur deskriptif smartphone yang akan digunakan untuk menghitung kemiripan konten.

  + Penghapusan Duplikasi Data Smartphone:
    
    Duplikasi pada cellphone_id dapat terjadi karena satu smartphone dinilai oleh banyak pengguna atau terdapat entri ganda dalam dataset data.csv. Penghapusan duplikat diperlukan untuk memastikan setiap smartphone unik memiliki satu representasi data.
    
    Dengan hanya mempertahankan entri unik berdasarkan cellphone_id, kita memastikan bahwa analisis karakteristik smartphone dan penghitungan kemiripan konten tidak terdistorsi oleh data yang berulang.

  + Ekstraksi dan Penggabungan Fitur Teks:
  
  Fitur-fitur penting seperti cellphone_id, brand, model, dan operating_system diekstraksi. Fitur-fitur teks (brand, model, operating system) kemudian digabungkan menjadi satu string tunggal (features) untuk setiap smartphone. Penggabungan ini penting untuk menciptakan "profil" konten setiap smartphone.
  
   DataFrame phone_dataset final berisi fitur-fitur inti yang cukup untuk membangun sistem rekomendasi berbasis content-based filtering. Kolom features inilah yang akan diolah lebih lanjut.
  
  + TF-IDF Vectorization:

    Metode Term Frequency-Inverse Document Frequency (TF-IDF) digunakan untuk mengubah teks fitur gabungan (features) ini menjadi representasi numerik (vektor) yang dapat diproses oleh algoritma. TF-IDF memberikan bobot lebih tinggi pada kata-kata yang penting dalam satu dokumen tetapi jarang muncul di dokumen lain. Hal ini membantu mengidentifikasi fitur unik yang membedakan satu smartphone dari yang lain dalam korpus.
    
    Proses ini menghasilkan tfidf_matrix di mana setiap baris merepresentasikan smartphone dan setiap kolom merepresentasikan bobot TF-IDF dari suatu kata kunci. Matriks tfidf_matrix ini kemudian digunakan untuk menghitung kesamaan antar smartphone di bagian Modeling.

    ![image](https://github.com/user-attachments/assets/ca11fc1b-79c3-4c59-9de0-05801bc59199)

+ Collaborative Filtering Data Preparation

Untuk pendekatan Collaborative Filtering, fokusnya adalah pada data interaksi pengguna-item (rating) untuk melatih model yang dapat memprediksi preferensi pengguna.

  + Pemetaan ID Pengguna dan Item:
    
  user_id dan cellphone_id asli (yang merupakan ID arbitrer) dikodekan ulang menjadi indeks numerik berurutan. Ini diperlukan karena model deep learning, khususnya embedding layer, memerlukan input integer berurutan sebagai indeks.
    
    Pemetaan ini menciptakan representasi integer padat untuk setiap pengguna dan smartphone, mengoptimalkan penggunaan memori dan komputasi dalam model neural network.
    
  + Normalisasi Rating:

  Rating asli (misalnya, dari 1-10) dinormalisasi ke skala 0-1. Normalisasi ini adalah praktik umum dalam deep learning untuk memastikan bahwa nilai input berada dalam rentang yang konsisten, yang dapat mempercepat konvergensi model dan meningkatkan stabilitas pelatihan.

  Rating yang dinormalisasi akan menjadi target output model Collaborative Filtering.
  
  + Data Split untuk Pelatihan dan Pengujian:

  Data rating yang telah diproses dibagi menjadi set pelatihan (training) dan pengujian (testing) untuk mengevaluasi kinerja model secara objektif. Pembagian dilakukan secara acak (random_state=42) dengan rasio 80% untuk pelatihan dan 20% untuk pengujian.

  Pembagian ini memastikan bahwa model dilatih pada satu subset data (X_train, y_train) dan dievaluasi pada subset data yang belum pernah dilihat sebelumnya (X_test, y_test) untuk mengukur kemampuan generalisasinya dan mendeteksi overfitting.

4. Hasil Utama Data Preparation

+ Peningkatan Kualitas Data:
  
  + Nol missing value - Data lengkap untuk semua fitur.
  + Penghapusan rating anomali - Konsistensi skala rating.
  + Standardisasi entri occupation - Format data seragam.
  + Eliminasi smartphone duplikat - Keunikan data produk, penting untuk Content-Based Filtering.
  + Format data konsisten - Siap untuk pemodelan.

+ Dataset Bersih Final:
  
  + Jumlah Record: 33 smartphone unik (untuk Content-Based), dan jumlah interaksi rating yang bersih setelah pra-pemrosesan (untuk Collaborative Filtering).
  + Fitur Content-Based: cellphone_id, brand, model, operating_system, dan features (gabungan teks).
  + Fitur Collaborative Filtering: user (encoded), cellphone (encoded), dan rating_normalized.
  + Kualitas: Sepenuhnya dibersihkan dan distandarisasi sesuai kebutuhan masing-masing pendekatan.
  + Kesiapan: Siap untuk pemodelan sistem rekomendasi.
    
5. Penggunaan

Dataset yang telah diproses siap untuk:

+ Content-based Filtering: Algoritma rekomendasi berdasarkan similarity fitur (menggunakan tfidf_matrix).
+ Collaborative Filtering: Pelatihan model neural network untuk prediksi rating (menggunakan X_train, y_train, X_test, y_test).
+ Analisis Similarity: Perbandingan karakteristik antar smartphone.
+ Feature Engineering: Pengembangan fitur lanjutan untuk model yang lebih kompleks (jika diperlukan).
  
Dataset ini optimal untuk sistem rekomendasi skala kecil hingga menengah dengan fokus pada karakteristik teknis smartphone dan pola interaksi pengguna.

## Modeling

1. Content-Based Filtering

+ Cosine Similarity:
  Setelah fitur teks smartphone diubah menjadi representasi numerik menggunakan TF-IDF Vectorization di tahap Data Preparation, langkah selanjutnya adalah menghitung kemiripan antar smartphone. Cosine Similarity adalah metrik yang umum digunakan untuk mengukur kemiripan antara dua vektor non-nol dalam ruang produk skalar. Nilai Cosine Similarity berkisar antara -1 (berlawanan sempurna) hingga 1 (sangat mirip sempurna). Dalam konteks ini, nilai yang lebih tinggi menunjukkan kemiripan konten yang lebih tinggi antara dua smartphone.

  cosine_sim adalah matriks berdimensi (N timesN), di mana N adalah jumlah smartphone unik (33 dalam kasus ini). Setiap elemen C_ij dalam matriks ini menunjukkan tingkat kemiripan konten antara smartphone i dan smartphone j.

+ Contoh Top-5 Recommendations

![image](https://github.com/user-attachments/assets/e4b4c57b-59ad-469f-bba6-09139287c8fd)

Rekomendasi menunjukkan bahwa smartphone dengan fitur dan merek serupa (Motorola Moto G series) memiliki kesamaan konten sangat tinggi, sementara perangkat dari merek lain seperti Samsung meskipun masih Android, memiliki kesamaan yang jauh lebih rendah. Hal ini mengindikasikan bahwa model Content-Based Filtering berhasil mengidentifikasi smartphone yang memiliki karakteristik serupa berdasarkan profil teks yang diolah.

2. Collaborative Filtering

Pendekatan Collaborative Filtering menggunakan Neural Network untuk memprediksi rating smartphone berdasarkan pola preferensi pengguna dan item similarity. Model ini berupaya mempelajari representasi (embedding) implisit untuk pengguna dan item dari data rating yang ada.

+ Arsitektur Model Deep Learning:
  
Model ini dirancang dengan arsitektur neural network untuk memprediksi rating pengguna, mengikuti paradigma Neural Collaborative Filtering (NCF). Arsitektur model terdiri dari:

  + Input Layer: Model menerima dua input terpisah: user_input (untuk ID pengguna) dan cellphone_input (untuk ID smartphone).
  + Embedding Layer:
    + user_embedding: Layer Embedding untuk pengguna. Dimensi input adalah num_users (jumlah pengguna unik) dan embedding_size adalah 50. Layer ini mengubah ID pengguna integer menjadi vektor dens berukuran 50 dimensi.
    + cellphone_embedding: Layer Embedding untuk smartphone. Dimensi input adalah num_cellphones (jumlah smartphone unik) dan embedding_size adalah 50. Layer ini mengubah ID smartphone integer menjadi vektor dens berukuran 50 dimensi.
  + Flatten Layer: Output dari setiap embedding layer diratakan (flatten) menjadi satu dimensi untuk dapat digabungkan.
  + Concatenate Layer: Vektor embedding yang diratakan dari pengguna (user_vec) dan item (cellphone_vec) digabungkan (concatenate) menjadi satu vektor fitur gabungan.
  + Dense Layers (Multi-Layer Perceptron):
    + Lapisan Dense pertama dengan 128 unit (neuron) dan fungsi aktivasi ReLU (Rectified Linear Unit), yang membantu memperkenalkan non-linearitas ke dalam model.
    + Lapisan Dropout dengan rate 0.2 diterapkan setelah lapisan dense pertama. Dropout adalah teknik regularisasi yang secara acak menonaktifkan sebagian neuron selama pelatihan, membantu mencegah overfitting.
    + Lapisan Dense kedua dengan 64 unit dan fungsi aktivasi ReLU.
  + Output Layer: Lapisan Dense terakhir dengan 1 unit dan fungsi aktivasi Sigmoid. Fungsi aktivasi Sigmoid digunakan karena target prediksi adalah rating yang dinormalisasi antara 0 dan 1.
  
+ Fungsi Loss, Optimizer, dan Parameter Pelatihan:

  + Fungsi Loss: Mean Squared Error (MSE) digunakan sebagai fungsi loss. MSE mengukur rata-rata kuadrat selisih antara rating prediksi dan rating aktual, yang cocok untuk masalah regresi (prediksi nilai rating kontinu).
  + Optimizer: Optimizer Adam digunakan untuk mengoptimalkan bobot model selama pelatihan. Adam adalah optimizer adaptif yang efisien dan sering memberikan performa baik dalam berbagai skenario deep learning. Learning rate diatur ke 0.001.
  + Epoch: Model dilatih selama 100 epoch. Epoch adalah satu siklus lengkap pelatihan di mana seluruh dataset pelatihan dilewatkan maju dan mundur melalui jaringan saraf satu kali.
  Batch Size: Setiap batch pelatihan berisi 64 sampel. Batch size menentukan jumlah sampel pelatihan yang akan dipropagasikan melalui jaringan sebelum parameter model diperbarui.
  + Early Stopping: Callback EarlyStopping diimplementasikan untuk memantau val_loss (loss pada data validasi) selama pelatihan. Jika val_loss tidak menunjukkan peningkatan (berkurang) selama patience (10 epoch) berturut-turut, pelatihan akan dihentikan lebih awal. restore_best_weights=True memastikan bahwa bobot model yang memiliki val_loss terendah selama pelatihan akan digunakan. Ini membantu mencegah overfitting dan menghemat waktu komputasi.

+ Contoh Top-5 Recommendations

![image](https://github.com/user-attachments/assets/1098face-7e51-481d-a539-8bc89b3ba627)

Rekomendasi untuk User ID 0 menunjukkan preferensi kuat terhadap Samsung Galaxy S22 (rating 9). Contoh ini mengilustrasikan bagaimana model Collaborative Filtering dapat memprediksi preferensi pengguna berdasarkan pola rating historis, dan merekomendasikan smartphone yang kemungkinan besar akan disukai pengguna berdasarkan kesamaan preferensi dengan pengguna lain.

3. Perbandingan Kedua Pendekatan

+ Content-Based Filtering
  
  + Kelebihan:
  
    + Memberikan rekomendasi berdasarkan spesifikasi teknis yang jelas.
    + Tidak memerlukan data pengguna lain (mengatasi cold start problem untuk item baru).
    + Dapat menjelaskan alasan rekomendasi dengan jelas (misalnya, "direkomendasikan karena mirip dengan smartphone X yang Anda sukai").
    + Konsisten dengan preferensi pengguna yang jelas.
    
  + Kekurangan:
  
    + Rekomendasi cenderung homogen (misalnya, hanya merekomendasikan smartphone dari brand dan OS yang sama).
    + Tidak dapat menangkap preferensi yang kompleks atau serendipitous (rekomendasi tak terduga yang disukai pengguna).
    + Terbatas pada fitur yang tersedia dalam metadata smartphone.
    
+ Collaborative Filtering

  + Kelebihan:
  
    + Dapat memberikan rekomendasi yang beragam dan mengejutkan (serendipitous).
    + Mampu menangkap pola preferensi yang kompleks yang tidak selalu eksplisit dalam fitur item.
    + Memanfaatkan wisdom of crowds (preferensi kolektif dari semua pengguna).
    + Adaptif terhadap perubahan preferensi pengguna seiring waktu (jika data rating diperbarui).
    
  Kekurangan:
  
    + Cold start problem untuk pengguna atau smartphone baru (memerlukan data rating yang cukup).
    + Memerlukan data rating yang banyak untuk performa optimal.
    + Model kompleks yang membutuhkan computational resources tinggi.
    + Sulit untuk diinterpretasi atau dijelaskan alasan di balik rekomendasinya.

## Evaluasi

1. Metrik Evaluasi yang Digunakan

Untuk mengukur kinerja kedua pendekatan sistem rekomendasi, metrik-metrik berikut digunakan:

+ Root Mean Squared Error (RMSE) - Collaborative Filtering

![image](https://github.com/user-attachments/assets/c536d901-02ac-49c3-b6e6-d4fde686f86c)

Cara Kerja:

Mengukur rata-rata kuadrat selisih antara rating aktual dan prediksi. Metrik ini memberikan penalti yang lebih besar pada kesalahan prediksi yang besar. Semakin rendah nilai RMSE, semakin baik performa model dalam memprediksi rating.

+ Mean Absolute Error (MAE) - Collaborative Filtering

![image](https://github.com/user-attachments/assets/69b14c45-9421-46de-b1be-83ff07a64c15)

Cara Kerja:

Mengukur rata-rata absolut selisih antara rating aktual dan prediksi. MAE lebih robust terhadap outlier dibandingkan RMSE. Nilai MAE memiliki interpretasi langsung dalam skala rating (1-5), menunjukkan rata-rata "seberapa jauh" prediksi dari nilai sebenarnya.

+ Brand Diversity - Content-Based Filtering

![image](https://github.com/user-attachments/assets/d6a1f749-41e3-4e00-a525-376aca7e42e9)

Cara Kerja:

Mengukur keberagaman merek smartphone dalam daftar rekomendasi yang diberikan. Nilai 1.0 berarti semua rekomendasi berasal dari merek yang berbeda, sementara nilai rendah menunjukkan kurangnya variasi merek. Metrik ini penting untuk menghindari over-specialization pada satu merek.

+ OS Diversity - Content-Based Filtering

![image](https://github.com/user-attachments/assets/007a05fa-e9e0-4bbc-a338-03417ad74470)

Cara Kerja:

Mengukur keberagaman sistem operasi (misalnya, Android, iOS) dalam daftar rekomendasi. Mirip dengan Brand Diversity, metrik ini mencegah bias yang terlalu kuat terhadap satu jenis sistem operasi, memastikan rekomendasi yang lebih bervariasi.

+ Average Similarity Score - Content-Based Filtering

![image](https://github.com/user-attachments/assets/11cd2815-43c1-4677-a4b2-32495e9b3102)

Cara Kerja:

Mengukur rata-rata kemiripan konten (berdasarkan Cosine Similarity) antara smartphone yang direkomendasikan. Nilai tinggi menunjukkan bahwa rekomendasi yang diberikan memiliki karakteristik konten yang sangat mirip. Metrik ini perlu diimbangi dengan metrik diversity untuk menghindari redundansi dan memastikan rekomendasi tetap relevan tetapi tidak monoton.

2. Hasil Evaluasi

+ Collaborative Filtering

  + RMSE: 3.1312 - Error prediksi rata-rata
  approx 3.13 poin dalam skala 1-5.
  + MAE: 2.4619 - Simpangan rata-rata
  approx 2.46 poin dari rating aktual.
  + Interpretasi: Performa model Collaborative Filtering menunjukkan akurasi prediksi yang moderat. Nilai RMSE dan MAE yang relatif tinggi mengindikasikan masih ada ruang yang signifikan untuk perbaikan dalam memprediksi rating pengguna secara tepat. Ini bisa disebabkan oleh sparsity data, kurangnya data rating yang bervariasi, atau kebutuhan untuk arsitektur model yang lebih kompleks.

+ Content-Based Filtering

  + Brand Diversity: 0.2727 - 27.27% rekomendasi dari merek yang berbeda.
  + OS Diversity: 0.1182 - 11.82% rekomendasi dari OS yang berbeda.
  + Average Similarity: 0.4721 - Tingkat kemiripan konten yang memadai.
  + Interpretasi: Sistem Content-Based Filtering berhasil dalam menemukan smartphone yang relevan secara konten, ditunjukkan oleh nilai Average Similarity yang cukup. Namun, nilai Brand Diversity (0.2727) dan OS Diversity (0.1182) yang rendah menunjukkan bahwa rekomendasi cenderung homogen, artinya sistem cenderung merekomendasikan smartphone dari merek dan sistem operasi yang sama atau sangat mirip dengan input pengguna. Hal ini adalah karakteristik umum dari Content-Based Filtering yang fokus pada kemiripan fitur.

3. Kesimpulan Evaluasi

+ Collaborative Filtering menunjukkan kemampuan prediksi yang dapat diterima namun memerlukan perbaikan untuk meningkatkan akurasi. Strategi peningkatan mungkin melibatkan penggunaan dataset yang lebih besar, fine-tuning hyperparameter model, atau eksplorasi arsitektur neural network yang lebih canggih.
+ Content-Based Filtering efektif dalam menemukan smartphone serupa berdasarkan karakteristiknya, tetapi perlu peningkatan diversity dalam rekomendasinya. Untuk mengatasi homogenitas, pendekatan hybrid atau strategi diversifikasi pasca-pemrosesan dapat dipertimbangkan.
+ Kombinasi kedua pendekatan (hybrid approach) di masa depan dapat mengoptimalkan kelebihan masing-masing sistem, menyediakan rekomendasi yang tidak hanya relevan tetapi juga beragam, sehingga meningkatkan pengalaman pengguna secara keseluruhan.
