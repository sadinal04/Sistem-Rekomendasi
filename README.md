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

Dataset yang digunakan dalam proyek ini merupakan **MovieLens (ml-latest)**, yang merupakan dataset populer untuk penelitian sistem rekomendasi. Dataset ini diperoleh dari Kaggle melalui tautan berikut: [MovieLens Dataset di Kaggle](https://www.kaggle.com/datasets/kanametov/movies-recomendation-system/data). Dataset ini berisi lebih dari 22 juta rating dari sekitar 247 ribu pengguna terhadap lebih dari 34 ribu film, yang dikumpulkan antara tahun 1995 hingga 2016.

Dataset terdiri dari beberapa file utama, pada proyek ini saya menggunakan dua file dataset yaitu:
- **`ratings.csv`**: berisi data rating film oleh pengguna dengan kolom `userId`, `movieId`, `rating`, dan `timestamp`.
- **`movies.csv`**: berisi informasi film seperti `movieId`, `title`, dan `genres`.

### Jumlah Baris dan Kolom Dataset

Dataset yang digunakan terdiri dari beberapa file dengan jumlah baris dan kolom sebagai berikut:
- **`ratings.csv`**: Memiliki **22,884,377 baris** dan **4 kolom** (`userId`, `movieId`, `rating`, `timestamp`).
- **`movies.csv`**: Memiliki **34,208 baris** dan **3 kolom** (`movieId`, `title`, `genres`).

### Kondisi Data (Missing Values)

Sebelum dilakukan preprocessing, data pada **`ratings.csv`** dan **`movies.csv`** telah diperiksa untuk memastikan kualitasnya. Berikut adalah hasil pemeriksaan missing values pada dataset:

1. **`ratings.csv`**:
   - Kolom **`userId`**, **`movieId`**, **`rating`**, dan **`timestamp`** tidak memiliki missing values. Semua kolom memiliki **0 missing values**.
   
2. **`movies.csv`**:
   - Kolom **`movieId`**, **`title`**, dan **`genres`** tidak memiliki missing values. Semua kolom memiliki **0 missing values**.

### Variabel dan Fitur Data

- **userId**: ID unik untuk tiap pengguna yang memberikan rating.  
- **movieId**: ID unik film, sebagai kunci untuk menghubungkan data antar file.  
- **rating**: Nilai rating film dari pengguna dalam skala 0.5 hingga 5.0, dengan interval setengah bintang.  
- **timestamp**: Waktu rating diberikan (dalam detik sejak epoch).  
- **title**: Judul film dengan tahun rilis, membantu identifikasi dan visualisasi.  
- **genres**: Daftar genre film yang dipisahkan dengan tanda pipe (`|`), misalnya "Action|Adventure|Comedy".

### Eksplorasi Data dan Insight

- **Distribusi Rating**  
  Mayoritas rating berada pada rentang 3 sampai 4 bintang, menunjukkan kecenderungan pengguna memberi nilai sedang hingga baik. Ini menunjukkan bahwa sebagian besar film yang direkomendasikan cenderung memiliki rating positif, namun tidak terlalu ekstrem.
  
- **Distribusi Genre Film**  
  Genre **Drama**, **Comedy**, dan **Action** mendominasi jumlah film di dataset, menandakan genre populer yang banyak diminati oleh pengguna. Hal ini juga mencerminkan pola umum dalam pilihan film yang sering disukai oleh pengguna.

## Data Preparation

Pada tahap ini dilakukan beberapa proses persiapan data untuk memastikan data yang digunakan sudah bersih, terstruktur, dan siap digunakan dalam pemodelan sistem rekomendasi.

### Tahapan Data Preparation yang Dilakukan

1. **Pembuatan Fitur 'Description'**  
   Sebuah fitur baru, yaitu **'description'**, dibuat dengan menggabungkan kolom `title` dan `genres` untuk menghasilkan deskripsi film yang lebih lengkap. Kolom `genres` yang berisi genre film dipisahkan oleh tanda pipe (`|`), sedangkan kolom `title` berisi judul film. Fitur ini berguna untuk meningkatkan pemahaman model mengenai konten film.

2. **Penggunaan TfidfVectorizer pada 'Description'**  
   Fitur **'description'** yang sudah dibuat kemudian diproses menggunakan **TfidfVectorizer** untuk mengubah teks menjadi matriks fitu## Data Preparation

Pada tahap ini, dilakukan beberapa proses persiapan data untuk memastikan data yang digunakan sudah bersih, terstruktur, dan siap digunakan dalam pemodelan sistem rekomendasi.

### Tahapan Data Preparation yang Dilakukan

1. **Pembuatan Fitur 'Description'**  
   Sebuah fitur baru, yaitu **'description'**, dibuat dengan menggabungkan kolom **`title`** dan **`genres`** untuk menghasilkan deskripsi film yang lebih lengkap. Kolom **`genres`** yang berisi genre film dipisahkan oleh tanda pipe (`|`), sedangkan kolom **`title`** berisi judul film. Fitur ini berguna untuk meningkatkan pemahaman model mengenai konten film, dan akan digunakan dalam **Content-Based Filtering**.

2. **Penggunaan TfidfVectorizer pada 'Description'**  
   Fitur **'description'** yang sudah dibuat kemudian diproses menggunakan **TfidfVectorizer** untuk mengubah teks deskripsi menjadi representasi numerik. TF-IDF (Term Frequency-Inverse Document Frequency) digunakan untuk menghitung skor pentingnya setiap kata dalam deskripsi film. Ini memungkinkan model untuk memanfaatkan informasi yang terkandung dalam teks deskripsi film dan memahami hubungan antar film berdasarkan kata-kata yang ada pada deskripsi tersebut.

3. **Konversi dan Transformasi Fitur Genre**  
   Kolom **`genres`** yang berupa string dengan genre yang dipisahkan tanda pipe (`|`) diubah menjadi **list** genre per film. Selanjutnya dilakukan **one-hot encoding** menggunakan **MultiLabelBinarizer** agar genre dapat direpresentasikan dalam bentuk vektor numerik yang dapat digunakan dalam model machine learning.

4. **Penggabungan Fitur One-Hot Encoding Genre dan Matriks TF-IDF Deskripsi**  
   Setelah proses **one-hot encoding genre** dan **TF-IDF** pada deskripsi, kedua fitur tersebut digabungkan menggunakan **`hstack`** (horizontal stack) untuk membentuk matriks fitur gabungan yang mencakup genre dan deskripsi film. Matriks ini digunakan sebagai input untuk model **Content-Based Filtering**.

5. **Mapping User dan Movie ke Indeks Numerik**  
   Karena model menggunakan embedding layer yang mengharuskan input berupa indeks numerik, **`userId`** dan **`movieId`** dari dataset dipetakan menjadi indeks berturut-turut mulai dari 0. Hal ini memudahkan pemodelan dan mengoptimalkan penggunaan memori pada model **Collaborative Filtering**.

6. **Pembuatan Matriks Genre (genre_feature) untuk Model Collaborative Filtering**  
   Setelah genre diubah menjadi **one-hot encoding**, matriks **genre_feature** dibuat untuk mencocokkan indeks film dengan genre yang relevan. Matriks ini selanjutnya akan digunakan sebagai input tambahan dalam model **Collaborative Filtering** untuk meningkatkan akurasi rekomendasi.

7. **Pembagian Data Train dan Test**  
   Dataset rating dibagi menjadi data **train** dan **test** dengan proporsi **80:20** secara acak, untuk memastikan evaluasi model dilakukan pada data yang belum pernah dilihat selama training.

8. **Normalisasi Rating**  
   Nilai rating yang awalnya berada di rentang 0.5 hingga 5.0 dinormalisasi ke rentang [0, 1] untuk membantu proses training model menjadi lebih stabil dan mempercepat konvergensi. Pembagian ini memastikan bahwa rating dapat diproses dalam model dengan cara yang lebih konsisten.

### Alasan Tahapan Data Preparation

- **Pembuatan fitur 'description'** memberikan informasi tambahan untuk model berbasis konten yang akan memproses data tekstual, memungkinkan pemodelan berdasarkan kemiripan konten (genre dan deskripsi film).
- **Penggunaan TfidfVectorizer** memungkinkan model untuk memanfaatkan informasi dari kolom deskripsi dan memahami kemiripan antar film berdasarkan kata-kata dalam deskripsi, sehingga meningkatkan kualitas rekomendasi berbasis konten.
- **Representasi numerik fitur genre** dibutuhkan agar data tekstual dapat diproses oleh model neural network. **One-hot encoding genre** memastikan bahwa genre dapat diterima sebagai input numerik yang mudah diolah.
- **Penggabungan genre dan deskripsi** dengan **hstack** memungkinkan pembuatan fitur gabungan yang menggabungkan dua aspek penting dari film: genre dan deskripsi. Hal ini menghasilkan fitur yang lebih kaya dan dapat digunakan untuk menghitung **cosine similarity** yang lebih akurat dalam **Content-Based Filtering**.
- **Mapping indeks numerik untuk user dan movie** adalah standar dalam pemodelan embedding sehingga input dapat diterima oleh layer embedding secara efisien.
- **Pembuatan matriks genre** untuk model **Collaborative Filtering** meningkatkan kualitas representasi genre film berdasarkan **embedding**, yang membantu model memahami hubungan antar film dengan lebih baik.
- **Split train-test** penting untuk melakukan evaluasi yang objektif dan menghindari overfitting pada model. Pembagian ini memastikan bahwa data yang digunakan untuk testing tidak digunakan saat training.
- **Normalisasi rating** membantu mencegah gradient yang tidak stabil dan mempercepat proses optimasi selama training, meningkatkan stabilitas model dan mempercepat konvergensi.

Tahapan ini memastikan data yang digunakan bersih, konsisten, dan sesuai format yang dibutuhkan oleh model sistem rekomendasi.

## Modeling

Pada tahap modeling, dua pendekatan sistem rekomendasi dikembangkan secara terpisah untuk menyelesaikan permasalahan dalam memberikan rekomendasi film yang relevan dan personal, yaitu **Content-Based Filtering** dan **Collaborative Filtering** berbasis **Neural Network**.

### 1. Content-Based Filtering

Pendekatan ini menggunakan fitur konten film seperti genre dan deskripsi untuk merekomendasikan film yang memiliki kemiripan tinggi dengan film yang disukai pengguna. Prosesnya meliputi:

- Ekstraksi fitur film dengan **one-hot encoding** pada genre dan representasi teks deskripsi menggunakan **TF-IDF** yang dilakukan pada Data Preparation.  
- Penghitungan similarity antar film menggunakan **cosine similarity**.  
- Rekomendasi diberikan berdasarkan film yang paling mirip dengan preferensi pengguna.

**Kelebihan:**  
- Tidak memerlukan data interaksi pengguna.  
- Interpretasi hasil rekomendasi lebih jelas karena didasarkan pada kemiripan fitur film.  

**Kekurangan:**  
- Terbatas pada fitur yang tersedia dan tidak dapat menangkap pola preferensi kompleks pengguna.  
- Cenderung merekomendasikan film yang serupa dan kurang bervariasi.

### Top-N Recommendation Output (Content-Based Filtering)

**Contoh Hasil Top-20 Recommendation (Content-Based Filtering)**  
Berikut adalah contoh rekomendasi film untuk **'Toy Story (1995)'** menggunakan **Content-Based Filtering** berdasarkan kemiripan konten:

1. **Toy Story 2 (1999)** (Similarity: 0.9786)  
2. **Toy Story Toons: Small Fry (2011)** (Similarity: 0.9300)  
3. **Toy Story Toons: Hawaiian Vacation (2011)** (Similarity: 0.9295)  
4. **Wild, The (2006)** (Similarity: 0.9030)  
5. **Monsters, Inc. (2001)** (Similarity: 0.8971)  
6. **Toy Story 3 (2010)** (Similarity: 0.8966)  
7. **Shrek the Third (2007)** (Similarity: 0.8936)  
8. **Turbo (2013)** (Similarity: 0.8930)  
9. **Aladdin (1992)** (Similarity: 0.8915)  
10. **Boxtrolls, The (2014)** (Similarity: 0.8901)  
11. **Antz (1998)** (Similarity: 0.8891)  
12. **Brother Bear 2 (2006)** (Similarity: 0.8876)  
13. **The Magic Crystal (2011)** (Similarity: 0.8868)  
14. **Tale of Despereaux, The (2008)** (Similarity: 0.8840)  
15. **Emperor's New Groove, The (2000)** (Similarity: 0.8805)  
16. **Adventures of Rocky and Bullwinkle, The (2000)** (Similarity: 0.8774)  
17. **DuckTales: The Movie - Treasure of the Lost Lamp (1990)** (Similarity: 0.8731)  
18. **Scooby-Doo! Mask of the Blue Falcon (2012)** (Similarity: 0.8730)  
19. **Asterix and the Vikings (Astérix et les Vikings) (2006)** (Similarity: 0.8626)  
20. **Home (2015)** (Similarity: 0.8299)  

Top-N rekomendasi dihasilkan berdasarkan **cosine similarity** antar film, yang menunjukkan seberapa mirip film yang disukai pengguna dengan film lain.

### 2. Collaborative Filtering dengan Neural Network

Pendekatan ini menggunakan data rating pengguna untuk mempelajari preferensi laten melalui **embedding user dan film**, serta mengintegrasikan fitur genre film sebagai input tambahan untuk memperkaya representasi film. Model neural network terdiri dari:

- **Embedding layer** untuk user dan film.  
- Input fitur genre sebagai vektor dense.  
- Beberapa lapisan **fully connected** dengan **dropout** untuk menghindari overfitting.  
- **Output layer** yang memprediksi rating film oleh pengguna.

**Kelebihan:**  
- Dapat menangkap pola interaksi kompleks antara pengguna dan film.  
- Memberikan rekomendasi yang lebih personal dan adaptif berdasarkan data nyata.

**Kekurangan:**  
- Membutuhkan data interaksi pengguna yang cukup lengkap.  
- Model lebih kompleks dan memerlukan waktu training lebih lama.

### Top-N Recommendation Output (Collaborative Filtering)

Setelah model dilatih, sistem menghasilkan rekomendasi film berupa daftar Top-N film dengan prediksi rating tertinggi untuk setiap pengguna, yang dapat langsung digunakan sebagai saran film yang sesuai dengan preferensi masing-masing pengguna.

**Contoh Hasil Top-5 Recommendation (Collaborative Filtering)**  
Berikut adalah contoh rekomendasi film untuk **User ID 1** menggunakan **Collaborative Filtering** berdasarkan **prediksi rating** yang dihasilkan oleh model:

| **Title**                                | **Genres**                                | **Predicted Rating** |
|------------------------------------------|-------------------------------------------|----------------------|
| Shawshank Redemption, The (1994)         | [Crime, Drama]                            | 0.830508             |
| Schindler's List (1993)                  | [Drama, War]                              | 0.826420             |
| Civil War, The (1990)                    | [Documentary, War]                        | 0.820559             |
| Duck Amuck (1953)                        | [Animation, Children, Comedy]             | 0.817200             |
| Can't Change the Meeting Place (1979)    | [Action, Crime]                           | 0.813960             |

**Top-N recommendation** dihasilkan berdasarkan **prediksi rating** oleh model **Collaborative Filtering**, yang mengindikasikan film-film yang memiliki kemungkinan besar untuk disukai oleh **User ID 1** berdasarkan data interaksi rating pengguna sebelumnya.

## Evaluation

Pada tahap evaluasi, evaluasi dilakukan menggunakan **Precision@K** Untuk **Content-Based Filtering**. Metrik **Root Mean Squared Error (RMSE)** digunakan untuk mengukur akurasi prediksi rating yang dihasilkan oleh model **Collaborative Filtering**. RMSE menghitung akar kuadrat dari rata-rata kuadrat selisih antara nilai rating asli dengan prediksi model, sehingga memberikan gambaran seberapa dekat prediksi model dengan data nyata.

### 1. Content-Based Filtering Evaluation

Untuk **Content-Based Filtering**, evaluasi dilakukan menggunakan **Precision@K** sebagai metrik utama. Pendekatan ini merekomendasikan film berdasarkan kemiripan konten, seperti genre dan deskripsi film. **Precision@K** mengukur seberapa banyak film yang relevan berada di dalam **Top K rekomendasi** yang dihasilkan oleh sistem.

#### **Precision@K dengan Threshold 0.7**

**Threshold 0.7** digunakan untuk **menentukan relevansi** film dalam perhitungan **Precision@K**. Dalam hal ini, **cosine similarity** antar film dihitung untuk menentukan seberapa mirip film yang direkomendasikan dengan film input. Film yang memiliki **cosine similarity lebih besar dari 0.7** dianggap **relevan** dan dimasukkan dalam perhitungan **Precision@K**. 

**Contoh Hasil Top-N Recommendation (Content-Based Filtering)**  
Berikut adalah contoh rekomendasi film untuk **'Toy Story (1995)'** menggunakan **Content-Based Filtering** berdasarkan kemiripan konten dengan **threshold 0.7** untuk **cosine similarity**:

| **Rank** | **Title**                        | **Similarity** |
|----------|----------------------------------|----------------|
| 1        | Toy Story 2 (1999)               | 0.9786         |
| 2        | Toy Story Toons: Small Fry (2011) | 0.9300         |
| 3        | Toy Story Toons: Hawaiian Vacation (2011) | 0.9295   |
| 4        | Wild, The (2006)                 | 0.9030         |
| 5        | Monsters, Inc. (2001)           | 0.8971         |
| 6        | Toy Story 3 (2010)              | 0.8966         |
| 7        | Shrek the Third (2007)          | 0.8936         |
| 8        | Turbo (2013)                    | 0.8930         |
| 9        | Aladdin (1992)                  | 0.8915         |
| 10       | Boxtrolls, The (2014)           | 0.8901         |
| 11       | Antz (1998)                     | 0.8891         |
| 12       | Brother Bear 2 (2006)           | 0.8876         |
| 13       | The Magic Crystal (2011)        | 0.8868         |
| 14       | Tale of Despereaux, The (2008)  | 0.8840         |
| 15       | Emperor's New Groove, The (2000)| 0.8805         |
| 16       | Adventures of Rocky and Bullwinkle, The (2000) | 0.8774 |
| 17       | DuckTales: The Movie - Treasure of the Lost Lamp (1990) | 0.8731 |
| 18       | Scooby-Doo! Mask of the Blue Falcon (2012) | 0.8730 |
| 19       | Asterix and the Vikings (Astérix et les Vikings) (2006) | 0.8626 |
| 20       | Home (2015)                     | 0.8299         |

**Precision@20: 1.0000** menunjukkan bahwa semua 20 film yang direkomendasikan sangat relevan berdasarkan kemiripan konten.

### 2. Collaborative Filtering Evaluation

Untuk **Collaborative Filtering**, evaluasi dilakukan menggunakan **Root Mean Squared Error (RMSE)**, yang mengukur akurasi prediksi rating berdasarkan data interaksi pengguna dengan film. RMSE memberikan gambaran seberapa besar kesalahan dalam prediksi rating yang diberikan oleh model dibandingkan dengan rating asli yang diberikan oleh pengguna.

#### **Formula RMSE**

$$
\text{RMSE} = \sqrt{\frac{1}{N} \sum_{i=1}^N (\hat{y}_i - y_i)^2}
$$

di mana:  
- $\(\hat{y}_i\)$ adalah prediksi rating untuk sampel ke-\(i\)  
- $\(y_i\)$ adalah rating asli untuk sampel ke-\(i\)  
- $\(N\)$ adalah jumlah total sampel yang dievaluasi

#### **Hasil Evaluasi RMSE**
- Model **Collaborative Filtering** yang dibangun berhasil mencapai **RMSE sekitar 0.18** pada data validasi, yang menunjukkan prediksi rating sangat mendekati rating asli pengguna dan model sudah cukup baik (berdasarkan training 5 epoch dengan RMSE validasi sekitar 0.18).

**Kelebihan:**  
- Dapat menangkap pola interaksi kompleks antara pengguna dan film.
- Memberikan rekomendasi yang lebih personal dan adaptif berdasarkan data nyata.

**Kekurangan:**  
- Membutuhkan data interaksi pengguna yang cukup lengkap.
- Model lebih kompleks dan memerlukan waktu training lebih lama.

### Kesimpulan
- **Content-Based Filtering** menggunakan **Precision@K** dengan threshold **0.7** untuk **cosine similarity** sebagai metrik evaluasi, yang menunjukkan bahwa semua rekomendasi yang diberikan sangat relevan dengan film input berdasarkan kemiripan konten.
- **Collaborative Filtering** menggunakan **RMSE** untuk mengukur kualitas prediksi rating yang diberikan oleh model.
