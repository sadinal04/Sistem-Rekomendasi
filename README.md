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

### Kondisi Data (Missing Values & Duplicate)

Sebelum dilakukan preprocessing, data pada **`ratings.csv`** dan **`movies.csv`** telah diperiksa untuk memastikan kualitasnya. Berikut adalah hasil pemeriksaan missing values dan duplikasi pada dataset:

1. **`ratings.csv`**:
   - Kolom **`userId`**, **`movieId`**, **`rating`**, dan **`timestamp`** tidak memiliki missing values. Semua kolom memiliki **0 missing values** dan **0 Duplicate**.
   
2. **`movies.csv`**:
   - Kolom **`movieId`**, **`title`**, dan **`genres`** tidak memiliki missing values. Semua kolom memiliki **0 missing values** dan **0 Duplicate**.

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

Pada tahap ini, dilakukan beberapa proses persiapan data untuk memastikan data yang digunakan sudah bersih, terstruktur, dan siap digunakan dalam pemodelan sistem rekomendasi. Data preparation dilakukan secara terpisah untuk **Content-Based Filtering** dan **Collaborative Filtering** dengan tahapan yang berbeda sesuai kebutuhan masing-masing pendekatan.

### Content-Based Filtering Data Preparation

#### 1. Pembuatan Fitur 'Description'
Sebuah fitur baru, yaitu **'description'**, dibuat dengan menggabungkan kolom **`title`** dan **`genres`** untuk menghasilkan deskripsi film yang lebih lengkap. Fitur ini berguna untuk meningkatkan pemahaman model mengenai konten film dan akan digunakan dalam **Content-Based Filtering**.

```python
movies['description'] = movies['title'] + " is a movie in genres " + movies['genres']
```

#### 2. Preprocessing dan Transformasi Fitur Genre
Langkah-langkah preprocessing genre dilakukan sebagai berikut:

##### a. Konversi Genre dari String ke List
Kolom **`genres`** yang berupa string dengan genre yang dipisahkan tanda pipe (`|`) diubah menjadi **list** genre per film untuk memudahkan pemrosesan selanjutnya.

```python
movies['genres'] = movies['genres'].apply(lambda x: x.split('|') if x else [])
```

##### b. One-Hot Encoding Genre
Dilakukan **one-hot encoding** menggunakan **MultiLabelBinarizer** agar genre dapat direpresentasikan dalam bentuk vektor numerik yang dapat digunakan dalam model machine learning.

```python
mlb = MultiLabelBinarizer()
genre_onehot = pd.DataFrame(mlb.fit_transform(movies['genres']),
                            columns=mlb.classes_,
                            index=movies.index)
```

#### 3. Ekstraksi Fitur Deskripsi dengan TF-IDF
Fitur **'description'** yang sudah dibuat kemudian diproses menggunakan **TfidfVectorizer** untuk mengubah teks deskripsi menjadi representasi numerik. TF-IDF (Term Frequency-Inverse Document Frequency) digunakan untuk menghitung skor pentingnya setiap kata dalam deskripsi film.

```python
tfidf = TfidfVectorizer(stop_words='english')
tfidf_matrix = tfidf.fit_transform(movies['description'])
```

#### 4. Penggabungan Fitur Genre dan Deskripsi
Setelah proses **one-hot encoding genre** dan **TF-IDF** pada deskripsi, kedua fitur tersebut digabungkan menggunakan **`hstack`** (horizontal stack) untuk membentuk matriks fitur gabungan yang mencakup genre dan deskripsi film.

```python
combined_features = hstack([genre_onehot.values, tfidf_matrix])
```

#### 5. Pembuatan Indeks untuk Pencarian Film
Dibuat indeks untuk mempermudah pencarian film berdasarkan judul.

```python
indices = pd.Series(movies.index, index=movies['title'])
```

#### 6. Perhitungan Cosine Similarity
Matriks cosine similarity dihitung berdasarkan fitur gabungan untuk mengukur kemiripan antar film.

```python
cosine_sim = cosine_similarity(combined_features, combined_features)
```

### Collaborative Filtering Data Preparation

#### 1. Preprocessing Genre untuk Collaborative Filtering
Serupa dengan Content-Based Filtering, genre diubah menjadi format list dan dilakukan one-hot encoding menggunakan MultiLabelBinarizer.

```python
movies['genres'] = movies['genres'].apply(lambda x: x.split('|') if x else [])
mlb = MultiLabelBinarizer()
genre_encoded = mlb.fit_transform(movies['genres'])
```

#### 2. Mapping User dan Movie ke Indeks Numerik
Karena model menggunakan embedding layer yang mengharuskan input berupa indeks numerik, **`userId`** dan **`movieId`** dari dataset dipetakan menjadi indeks berturut-turut mulai dari 0.

```python
user_ids = ratings['userId'].unique().tolist()
movie_ids = ratings['movieId'].unique().tolist()

user2user_encoded = {x: i for i, x in enumerate(user_ids)}
movie2movie_encoded = {x: i for i, x in enumerate(movie_ids)}

ratings['user'] = ratings['userId'].map(user2user_encoded)
ratings['movie'] = ratings['movieId'].map(movie2movie_encoded)
```

#### 3. Pembuatan Matriks Genre untuk Model Collaborative Filtering
Setelah genre diubah menjadi **one-hot encoding**, matriks **genre_feature** dibuat untuk mencocokkan indeks film dengan genre yang relevan. Matriks ini akan digunakan sebagai input tambahan dalam model **Collaborative Filtering**.

```python
movie_id_reverse = {v: k for k, v in movie2movie_encoded.items()}
num_genres = genre_encoded.shape[1]
genre_feature = np.zeros((num_movies, num_genres))

for i in range(num_movies):
    movie_id = movie_id_reverse[i]
    idx = movies[movies['movieId'] == movie_id].index[0]
    genre_feature[i] = genre_encoded[idx]
```

#### 4. Pembagian Data Train dan Test
Dataset rating dibagi menjadi data **train** dan **test** dengan proporsi **80:20** secara acak menggunakan `train_test_split` untuk memastikan evaluasi model dilakukan pada data yang belum pernah dilihat selama training.

```python
train, test = train_test_split(ratings, test_size=0.2, random_state=42)
```

#### 5. Normalisasi Rating
Nilai rating yang awalnya berada di rentang 0.5 hingga 5.0 dinormalisasi ke rentang [0, 1] untuk membantu proses training model menjadi lebih stabil dan mempercepat konvergensi.

```python
min_rating = ratings['rating'].min()
max_rating = ratings['rating'].max()

train['rating_scaled'] = (train['rating'] - min_rating) / (max_rating - min_rating)
test['rating_scaled'] = (test['rating'] - min_rating) / (max_rating - min_rating)
```

#### 6. Penyiapan Data Genre untuk Training dan Testing
Genre feature disiapkan untuk data train dan test sesuai dengan indeks movie yang ada pada masing-masing dataset.

```python
train_genre = genre_feature[train['movie'].values]
test_genre = genre_feature[test['movie'].values]
```

### Alasan Tahapan Data Preparation

#### Content-Based Filtering

1. **Pembuatan fitur 'description'** : Menggabungkan informasi judul dan genre untuk memperkaya representasi konten film.

2. **Konversi genre ke list** : Mempermudah pemrosesan data genre yang awalnya berupa string.

3. **One-hot encoding genre** : Mengubah data genre menjadi format numerik yang bisa digunakan oleh model.

4. **Ekstraksi deskripsi dengan TF-IDF** : Mengubah teks deskripsi menjadi vektor numerik berdasarkan frekuensi kata.

5. **Penggabungan fitur genre dan deskripsi** : Menghasilkan fitur gabungan yang lebih informatif untuk menghitung kemiripan film.

6. **Pembuatan indeks pencarian film** : Memudahkan pencarian cepat film berdasarkan judul.

7. **Perhitungan cosine similarity** : Mengukur kemiripan antar film berdasarkan fitur gabungan.

---

#### Collaborative Filtering

1. **Preprocessing genre** : Mengubah genre menjadi format yang bisa digunakan untuk fitur tambahan.

2. **Mapping user dan movie ke indeks** : Menyesuaikan data agar dapat digunakan dalam layer embedding.

3. **Pembuatan matriks genre** : Menyediakan input fitur genre untuk setiap film dalam model.

4. **Pembagian data train dan test** : Memastikan evaluasi model dilakukan pada data yang belum dilatih.

5. **Normalisasi rating** : Menstabilkan proses training dan mempercepat konvergensi model.

6. **Penyiapan data genre untuk training dan testing** : Memastikan genre film tersedia sesuai dengan data yang digunakan untuk training dan testing.

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
