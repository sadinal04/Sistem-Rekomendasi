# Laporan Proyek Machine Learning - Sadinal Mufti

## Project Overview

Pada era digital saat ini, jumlah film dan konten hiburan yang tersedia secara online semakin bertambah pesat. Hal ini membuat pengguna kesulitan dalam memilih film yang sesuai dengan preferensi dan minat mereka. Sistem rekomendasi hadir sebagai solusi penting untuk mempersonalisasi pengalaman pengguna dengan menyajikan rekomendasi film yang relevan dan menarik.

Proyek ini bertujuan mengembangkan sistem rekomendasi film yang mampu memberikan saran film secara personal berdasarkan dua pendekatan utama: Content-Based Filtering dan Collaborative Filtering. Pendekatan ini memungkinkan sistem untuk merekomendasikan film tidak hanya berdasarkan kesamaan konten, tetapi juga pola interaksi pengguna dengan film.

Rekomendasi yang efektif dapat meningkatkan kepuasan pengguna, memperpanjang waktu penggunaan platform, serta meningkatkan engagement dan potensi pendapatan bagi penyedia layanan hiburan.

Berbagai riset menunjukkan bahwa sistem rekomendasi adalah komponen krusial dalam platform streaming dan e-commerce untuk meningkatkan pengalaman pengguna dan penjualan produk (Ricci et al., 2015)[1]. Model Collaborative Filtering dan Content-Based Filtering adalah metode populer yang telah terbukti efektif dalam domain rekomendasi (Adomavicius & Tuzhilin, 2005)[2].


**Referensi:**

[1] F. Ricci, L. Rokach, B. Shapira, and P. B. Kantor, *Recommender Systems Handbook*, 2nd ed. Springer, 2015.  
[Online]. Available: https://link.springer.com/book/10.1007/978-1-4899-7637-6

[2] G. Adomavicius and A. Tuzhilin, “Toward the next generation of recommender systems: A survey of the state-of-the-art and possible extensions,” *IEEE Transactions on Knowledge and Data Engineering*, vol. 17, no. 6, pp. 734–749, 2005.  
[Online]. Available: https://ieeexplore.ieee.org/document/1423975

## Business Understanding

Pada bagian ini dilakukan proses klarifikasi masalah untuk memahami kebutuhan dan tantangan dalam mengembangkan sistem rekomendasi film yang efektif dan sesuai dengan preferensi pengguna.

### Problem Statements

- Bagaimana menyediakan rekomendasi film yang relevan dan personal berdasarkan informasi konten film seperti genre dan deskripsi?  
- Bagaimana menyediakan rekomendasi film yang relevan dan personal berdasarkan pola perilaku dan preferensi pengguna yang terekam melalui rating?

### Goals

- Membangun sistem rekomendasi yang dapat memberikan rekomendasi film personal berdasarkan konten film seperti genre dan deskripsi, sehingga pengguna dapat menemukan film serupa yang sesuai preferensi mereka.  
- Mengembangkan sistem rekomendasi yang memanfaatkan data rating dan interaksi pengguna dengan film untuk memprediksi preferensi pengguna secara lebih akurat melalui model collaborative filtering.  

### Solution Approach

Untuk mencapai tujuan di atas, dua pendekatan sistem rekomendasi diterapkan secara terpisah dengan karakteristik berikut:

- **Content-Based Filtering**  
  Pendekatan ini hanya menggunakan informasi konten film, seperti genre dan deskripsi, untuk merekomendasikan film yang serupa. Karena tidak bergantung pada data interaksi pengguna, metode ini dapat diterapkan bahkan jika data pengguna tidak tersedia atau sangat terbatas.

- **Collaborative Filtering**  
  Pendekatan ini memprediksi preferensi pengguna terhadap film dengan mempelajari pola rating melalui embedding user dan film, serta melibatkan fitur genre sebagai input tambahan. Metode ini efektif untuk menangkap preferensi kompleks dari data interaksi dan dapat memberikan rekomendasi yang lebih personal dan adaptif.

Dengan mengembangkan kedua pendekatan ini secara terpisah, proyek ini dapat mengevaluasi kelebihan dan kekurangan masing-masing metode serta memahami konteks aplikasi terbaik bagi setiap pendekatan.

## Data Understanding

Dataset yang digunakan dalam proyek ini merupakan MovieLens (ml-latest), yang merupakan dataset populer untuk penelitian sistem rekomendasi. Dataset ini diperoleh dari Kaggle melalui tautan berikut: [MovieLens Dataset di Kaggle](https://www.kaggle.com/datasets/kanametov/movies-recomendation-system/data). Dataset ini berisi lebih dari 22 juta rating dari sekitar 247 ribu pengguna terhadap lebih dari 34 ribu film, yang dikumpulkan antara tahun 1995 hingga 2016.

Dataset terdiri dari beberapa file utama:
- `ratings.csv`: berisi data rating film oleh pengguna dengan kolom `userId`, `movieId`, `rating`, dan `timestamp`.  
- `movies.csv`: berisi informasi film seperti `movieId`, `title`, dan `genres`.  
- `tags.csv`: berisi metadata tagging film oleh pengguna.  
- `links.csv`: berisi penghubung ID film ke database lain seperti IMDb dan TMDb.

### Variabel dan Fitur Data

- **userId**: ID unik untuk tiap pengguna yang memberikan rating.  
- **movieId**: ID unik film, sebagai kunci untuk menghubungkan data antar file.  
- **rating**: nilai rating film dari pengguna dalam skala 0.5 hingga 5.0, dengan interval setengah bintang.  
- **timestamp**: waktu rating diberikan (dalam detik sejak epoch).  
- **title**: judul film dengan tahun rilis, membantu identifikasi dan visualisasi.  
- **genres**: daftar genre film yang dipisahkan dengan tanda pipe (`|`), misalnya "Action|Adventure|Comedy".

### Eksplorasi Data dan Insight

- **Distribusi Rating**  
  Mayoritas rating berada pada rentang 3 sampai 4 bintang, menunjukkan kecenderungan pengguna memberi nilai sedang hingga baik.  
- **Distribusi Genre Film**  
  Genre Drama, Comedy, dan Action mendominasi jumlah film di dataset, menandakan genre populer yang banyak diminati.

## Data Preparation

Pada tahap ini dilakukan beberapa proses persiapan data untuk memastikan data yang digunakan sudah bersih, terstruktur, dan siap digunakan dalam pemodelan sistem rekomendasi.

### Tahapan Data Preparation yang Dilakukan

1. **Pengisian dan Penanganan Nilai Kosong**  
   Pada kolom `genres` di file movies.csv, nilai kosong (missing values) diisi dengan string kosong (`''`) untuk menghindari error saat pemrosesan fitur genre.

2. **Konversi dan Transformasi Fitur Genre**  
   Kolom `genres` yang berupa string dengan genre yang dipisahkan tanda pipe (`|`) diubah menjadi list genre per film. Selanjutnya dilakukan one-hot encoding menggunakan `MultiLabelBinarizer` agar genre dapat direpresentasikan dalam bentuk vektor numerik yang dapat digunakan dalam model machine learning.

3. **Mapping User dan Movie ke Indeks Numerik**  
   Karena model menggunakan embedding layer yang mengharuskan input berupa indeks numerik, `userId` dan `movieId` dari dataset dipetakan menjadi indeks berturut-turut mulai dari 0. Hal ini memudahkan pemodelan dan mengoptimalkan penggunaan memori.

4. **Normalisasi Rating**  
   Nilai rating yang awalnya berada di rentang 0.5 hingga 5.0 dinormalisasi ke rentang [0, 1] untuk membantu proses training model menjadi lebih stabil dan mempercepat konvergensi.

5. **Pembagian Data Train dan Test**  
   Dataset rating dibagi menjadi data train dan test dengan proporsi 80:20 secara acak, untuk memastikan evaluasi model dilakukan pada data yang belum pernah dilihat selama training.

### Alasan Tahapan Data Preparation

- **Mengatasi missing values** pada fitur genre agar tidak terjadi error saat pengolahan fitur dan model training.  
- **Representasi numerik fitur genre** dibutuhkan agar data tekstual dapat diproses oleh model neural network.  
- **Mapping indeks numerik untuk user dan movie** adalah standar dalam pemodelan embedding sehingga input dapat diterima oleh layer embedding secara efisien.  
- **Normalisasi rating** membantu mencegah gradient yang tidak stabil dan mempercepat proses optimasi selama training.  
- **Split train-test** penting untuk melakukan evaluasi yang objektif dan menghindari overfitting pada model.

Tahapan ini memastikan data yang digunakan bersih, konsisten, dan sesuai format yang dibutuhkan oleh model sistem rekomendasi.

## Modeling

Pada tahap modeling, dua pendekatan sistem rekomendasi dikembangkan secara terpisah untuk menyelesaikan permasalahan dalam memberikan rekomendasi film yang relevan dan personal, yaitu Content-Based Filtering dan Collaborative Filtering berbasis Neural Network.

### 1. Content-Based Filtering

Pendekatan ini menggunakan fitur konten film seperti genre dan deskripsi untuk merekomendasikan film yang memiliki kemiripan tinggi dengan film yang disukai pengguna. Prosesnya meliputi:

- Ekstraksi fitur film dengan one-hot encoding pada genre dan representasi teks deskripsi menggunakan TF-IDF.  
- Penghitungan similarity antar film menggunakan cosine similarity.  
- Rekomendasi diberikan berdasarkan film yang paling mirip dengan preferensi pengguna.

**Kelebihan:**  
- Tidak memerlukan data interaksi pengguna.  
- Interpretasi hasil rekomendasi lebih jelas karena didasarkan pada kemiripan fitur film.  

**Kekurangan:**  
- Terbatas pada fitur yang tersedia dan tidak dapat menangkap pola preferensi kompleks pengguna.  
- Cenderung merekomendasikan film yang serupa dan kurang bervariasi.

### 2. Collaborative Filtering dengan Neural Network

Pendekatan ini menggunakan data rating pengguna untuk mempelajari preferensi laten melalui embedding user dan film, serta mengintegrasikan fitur genre film sebagai input tambahan untuk memperkaya representasi film. Model neural network terdiri dari:

- Embedding layer untuk user dan film.  
- Input fitur genre sebagai vektor dense.  
- Beberapa lapisan fully connected dengan dropout untuk menghindari overfitting.  
- Output layer yang memprediksi rating film oleh pengguna.

**Kelebihan:**  
- Dapat menangkap pola interaksi kompleks antara pengguna dan film.  
- Memberikan rekomendasi yang lebih personal dan adaptif berdasarkan data nyata.

**Kekurangan:**  
- Membutuhkan data interaksi pengguna yang cukup lengkap.  
- Model lebih kompleks dan memerlukan waktu training lebih lama.

### Top-N Recommendation Output

Setelah model dilatih, sistem menghasilkan rekomendasi film berupa daftar Top-N film dengan prediksi rating tertinggi untuk setiap pengguna, yang dapat langsung digunakan sebagai saran film yang sesuai dengan preferensi masing-masing pengguna.

## Evaluation

Pada tahap evaluasi, digunakan metrik **Root Mean Squared Error (RMSE)** untuk mengukur akurasi prediksi rating yang dihasilkan oleh model collaborative filtering. RMSE menghitung akar kuadrat dari rata-rata kuadrat selisih antara nilai rating asli dengan prediksi model, sehingga memberikan gambaran seberapa dekat prediksi model dengan data nyata.

### Formula RMSE

$$
\text{RMSE} = \sqrt{\frac{1}{N} \sum_{i=1}^N (\hat{y}_i - y_i)^2}
$$

di mana:  
- \(\hat{y}_i\) adalah prediksi rating untuk sampel ke-\(i\)  
- \(y_i\) adalah rating asli untuk sampel ke-\(i\)  
- \(N\) adalah jumlah total sampel yang dievaluasi

### Penjelasan Cara Kerja RMSE

- RMSE memberikan bobot lebih besar pada kesalahan prediksi yang besar karena menggunakan kuadrat selisih.  
- Nilai RMSE yang lebih kecil menunjukkan model dengan prediksi yang lebih akurat.  
- RMSE cocok digunakan pada kasus prediksi nilai kontinu seperti rating film.

### Hasil Evaluasi Proyek

- Model **Collaborative Filtering** yang dibangun berhasil mencapai RMSE sekitar **0.18** pada data validasi, yang menunjukkan prediksi rating sangat mendekati rating asli pengguna dan model sudah cukup baik (berdasarkan training 5 epoch dengan RMSE val sekitar 0.18).  
- Sedangkan model **Content-Based Filtering** tidak menggunakan metrik RMSE karena tidak memprediksi rating, melainkan merekomendasikan film berdasarkan kemiripan fitur konten. Evaluasi pada metode ini dilakukan dengan melihat **nilai cosine similarity** antara film input dengan film rekomendasi.  
- Rekomendasi yang dihasilkan memiliki skor cosine similarity tinggi (misal > 0.8), menunjukkan film-film yang direkomendasikan sangat relevan secara konten dan genre dengan film input.  
- Kedua pendekatan ini saling melengkapi: Collaborative Filtering menangkap preferensi pengguna dari data interaksi, sementara Content-Based Filtering menyediakan rekomendasi saat data interaksi kurang atau untuk film baru (cold start).

---

Penggunaan RMSE sangat tepat untuk mengukur performa model prediksi rating pada Collaborative Filtering, sedangkan evaluasi Content-Based Filtering lebih mengandalkan analisis kualitas rekomendasi berdasarkan kemiripan konten (cosine similarity) dan relevansi hasil rekomendasi.
