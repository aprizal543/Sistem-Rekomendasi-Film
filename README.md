# Laporan Proyek Machine Learning - Aprizal
## Domain Proyek
#### Latar Belakang
Di era digital saat ini, platform streaming film dan layanan hiburan menyediakan akses ke ribuan hingga jutaan konten film. Namun, kelimpahan pilihan ini sering kali memicu fenomena yang dikenal sebagai information overload, di mana pengguna merasa kewalahan dan kesulitan menentukan film yang sesuai dengan selera mereka. Tanpa adanya sistem yang memandu, pengguna mungkin menghabiskan lebih banyak waktu untuk mencari (scrolling) daripada menonton, yang pada akhirnya dapat menurunkan tingkat kepuasan dan retensi pengguna terhadap platform tersebut.

Sistem rekomendasi hadir sebagai solusi untuk mengatasi masalah ini dengan menyaring informasi dan memprediksi preferensi pengguna terhadap item tertentu. Proyek ini bertujuan untuk membangun sistem rekomendasi film dengan pendekatan ***Collaborative Filtering*** menggunakan teknik ***Neural Collaborative Filtering (NCF)***. Metode ini memodernisasi pendekatan Matrix Factorization klasik dengan memanfaatkan arsitektur Deep Learning (Neural Networks) untuk menangkap interaksi non-linear yang kompleks antara pengguna (user) dan item (movie) [1].

Pentingnya proyek ini terletak pada kemampuannya untuk memberikan pengalaman yang terpersonalisasi. Riset menunjukkan bahwa algoritma rekomendasi yang akurat dapat meningkatkan user engagement dan pendapatan platform secara signifikan dengan menyajikan konten yang relevan secara tepat waktu [2].
## Business Understanding
### Problem Statements
Berdasarkan latar belakang di atas, berikut adalah perincian masalah yang akan diselesaikan:
- Bagaimana cara mengolah data rating film untuk memprediksi preferensi pengguna terhadap film yang belum pernah mereka tonton?
- Bagaimana cara menghasilkan daftar rekomendasi Top-N film yang paling relevan dan personal bagi setiap pengguna?
- Bagaimana performa arsitektur berbasis Deep Learning (Neural Network) dibandingkan dengan metode statistik konvensional dalam mengukur akurasi rekomendasi?
### Goals
Tujuan utama dari pelaksanaan proyek ini adalah:
- Membangun model sistem rekomendasi yang mampu memprediksi rating pengguna dengan tingkat kesalahan (error) seminimal mungkin.
- Menghasilkan daftar rekomendasi film (Top-N Recommendation) yang akurat dan sesuai dengan selera historis pengguna.
- Mengimplementasikan teknik *Neural Collaborative Filtering* untuk menangkap pola preferensi pengguna yang kompleks.
### Solution Statements
Untuk mencapai tujuan tersebut, pendekatan solusi yang diajukan dalam kerangka berpikir sistem rekomendasi yaitu:
##### Collaborative Filtering:
  - **Deskripsi**: Pendekatan ini merekomendasikan item berdasarkan kemiripan pola interaksi antar pengguna. Asumsinya, jika Pengguna A dan Pengguna B memiliki selera yang sama pada masa lalu, mereka akan memiliki selera yang sama di masa depan.
  - **Cara Kerja**: Proyek ini menggunakan Neural *Collaborative Filtering (NCF)*. Data user dan movie dipetakan ke dalam vektor representasi (embeddings). Vektor ini kemudian diproses melalui lapisan neural network untuk memprediksi rating yang mungkin diberikan pengguna terhadap film tertentu.
  - **Kelebihan**: Tidak memerlukan metadata film yang detail (hanya butuh ID dan rating), mampu menemukan pola tersembunyi (latent factors), dan umumnya memiliki akurasi yang lebih tinggi untuk dataset besar.
### Data Understanding
Dataset yang digunakan dalam proyek ini adalah MovieLens Small Dataset. Dataset ini merupakan versi kecil dari dataset MovieLens yang sering digunakan sebagai standar benchmark dalam penelitian sistem rekomendasi.
- Sumber Data: website movielens (Tautan: [Dataset MovieLens](https://grouplens.org/datasets/movielens/)). 
- Jumlah Data:
   - Variabel ratings : Terdapat 4 kolom dan 100836 baris. 
   - Variabel movies : terdapat 3 kolom dan 9742 baris.
- Format: CSV / Excel.
##### Variabel pada Data
Proyek ini menggunakan dua file utama, yaitu ratings.csv dan movies.csv. Berikut adalah uraian variabelnya:
##### 1. ratings.csv
- userId : ID unik untuk setiap pengguna, type data (int64).
- movieId : ID unik untuk setiap film, type data (int64).
- rating : Penilaian pengguna terhadap film (skala 0.5 - 5.0), type data (float64).
- timestamps : waktu ketika rating diberikan, type data (int64)
##### 2. movies.csv
- movieId : ID unik film(sama seperti di variabel df_ratings), type data int(64).
- title : Judul film beserta tahun rilisnya, type data (object). 
- genres : genre film, type data (object).
##### Tahapan Pemahaman Data (EDA)
Untuk memahami data, langkah-langkah berikut dilakukan:
##### Visualisasi Distribusi Rating Film keseluruhan
<img width="594" height="276" alt="Distribusi Rating Film Keseluruhan" src="https://github.com/user-attachments/assets/4d550787-115c-4aeb-9cfa-a2017366bd08" />

###### Interpretasi & Insight:
-   **Kecenderungan Rating Tinggi** : Histogram distribusi rating menunjukkan bahwa sebagian besar pengguna cenderung memberikan rating yang tinggi (di atas 3.0), dengan puncak signifikan pada rating 4.0 dan 5.0. Rating 0.5 dan 1.0 relatif jarang. Ini mengindikasikan bahwa pengguna cenderung memberikan feedback positif atau mungkin lebih sering menilai film yang mereka nikmati.
##### Visualisasi Distribusi Film Paling Banyak Dinilai
<img width="671" height="275" alt="Top 10 film Paling banyak dinilai" src="https://github.com/user-attachments/assets/968168c2-1b64-40c0-99d3-dd770a35a2c1" />

###### Interpretasi & Insight:
- **Film Paling Populer** : Film-film seperti 'Forrest Gump', 'Shawshank Redemption', dan 'Pulp Fiction' menerima jumlah rating tertinggi, menunjukkan popularitas luas dan keterlibatan pengguna yang tinggi. Ini adalah film-film yang mungkin paling dikenal atau banyak ditonton oleh basis pengguna.
##### Visualisasi Distribusi Film dengan Rating Rata-rata Tertinggi
<img width="595" height="319" alt="Top 10 film dengan rating rata-rata tertinggi" src="https://github.com/user-attachments/assets/e7e84228-b69c-4848-a89c-9eebbbf66507" />

###### Interpretasi & Insight:
- **Film dengan Rating Rata-rata Tertinggi (dengan ambang batas minimal 50 rating)**: Film-film seperti 'Shawshank Redemption', 'Godfather, The', dan 'Fight Club' memiliki rating rata-rata tertinggi. Kualitas film-film ini dianggap sangat baik oleh sebagian besar penilai. Ambang batas minimal 50 rating membantu memastikan bahwa rating rata-rata ini lebih robust dan tidak bias oleh sedikit rating yang sangat tinggi.
##### Visualisasi Distribusi Jumlah Rating per Pengguna
<img width="584" height="275" alt="Distribusi jumlah rating per pengguna" src="https://github.com/user-attachments/assets/56ea0e9e-68d8-4a60-8af2-8357d4a2fd3a" />

###### Interpretasi & Insight:
- **Aktivitas Pengguna Bervariasi** : Histogram jumlah rating per pengguna menunjukkan distribusi yang condong ke kanan, dengan sebagian besar pengguna memberikan jumlah rating yang relatif kecil (sekitar 10-50 rating). Namun, ada juga sejumlah kecil pengguna yang sangat aktif, memberikan ratusan, bahkan lebih dari seribu rating. Hal ini mengindikasikan adanya kelompok kecil power-user yang berkontribusi signifikan terhadap data rating.
##### Visualisasi Distribusi Genre Paling Populer
<img width="682" height="271" alt="Top 10 genre paling populer" src="https://github.com/user-attachments/assets/ba0a350d-6719-4cbb-ae9f-edd07aeacd62" />

###### Interpretasi & Insight:
- **Genre Paling Populer (Jumlah Film)** : Genre 'Drama', 'Comedy', dan 'Thriller' adalah genre yang paling banyak muncul dalam dataset film. Ini menunjukkan preferensi umum atau ketersediaan film yang lebih banyak dalam genre-genre tersebut.
##### Visualisasi Distribusi Rating Rata-rata per Genre
<img width="673" height="270" alt="Rating rata-rata per genre" src="https://github.com/user-attachments/assets/6a6b40e8-ff35-4af7-ac53-08a42f4688f5" />

###### Interpretasi & Insight:
- **Rating Rata-rata per Genre** : Genre 'Film-Noir', 'War', dan 'Documentary' memiliki rating rata-rata tertinggi. Ini menunjukkan bahwa meskipun mungkin tidak sebanyak film dalam genre Comedy atau Drama, film-film dalam genre ini cenderung lebih disukai atau dianggap berkualitas tinggi oleh penilai. Genre seperti 'Children' dan 'Fantasy' memiliki rating rata-rata yang sedikit lebih rendah, tetapi masih cukup baik.
### Data Preparation
Tahapan data preparation sangat krusial untuk memastikan data siap dilatih oleh model. Proses yang dilakukan secara berurutan adalah:
1. **Data Cleaning (Pembersihan Data)**
    - Penanganan Missing Values: tidak ada satupun terdapat missing values pada datasetnya.
    - Penanganan Data Duplicate : tidak ada terdapat duplicate pada dataset tersebut.
2. **Encoding (Label Encoding)**
    - Proses : Mengubah userId dan movieId menjadi indeks berurutan(0 sampai N-1)
    - Alasan : ID asli pada dataset sering kali tidak berurutan (misal: ada lonjakan dari ID 100 langsung ke 150). Model Neural Network, khususnya layer Embedding, memerlukan input berupa integer yang padat dan berurutan agar dapat memetakan setiap ID ke vektor bobot secara efisien tanpa membuang memori untuk indeks yang kosong.
3. **Normalization(Skala Rating)**
    - Proses : Mengonversi nilai target rating (0.5 - 5.0) menjadi rentang [0, 1] menggunakan Min-Max Scaling.
    - Alasan : Aktivasi Sigmoid pada output layer model Neural Network bekerja pada rentang 0 hingga 1. Normalisasi membantu mempercepat konvergensi gradien saat pelatihan dan memastikan skala output model sesuai dengan fungsi aktivasi.
4. **Data Splitting (Pembagian Data)**
    - Proses: Membagi dataset menjadi Train Set (80%) dan Test Set (20%).
    - Alasan: Diperlukan untuk mengevaluasi performa model pada data yang belum pernah dilihat sebelumnya (unseen data) guna menghindari overfitting.
### Modeling and Result
Pada tahap ini, model sistem rekomendasi dibangun menggunakan pendekatan Deep Learning dengan library TensorFlow/Keras. Kelas model yang digunakan adalah RecommenderNet.
##### Skema Model : 
1. **Input Layer** : Menerima input berupa user_encoded dan movie_encoded.
2. **Embedding Layer** : 
    - Setiap user dan movie dipetakan ke dalam vektor dimensi rendah(yang digunakan 50 dimensi).
    - Fungsi : Embedding ini mempelajari representasu fitur laten dari pengguna dan karakteristik film selama proses pelatihan.
3. **Dot Product & Bias** :
    - Melakukan operasi dot product antara vektor user dan vektor movie.
    - Menambahkan bias user dan bias movie untuk menangkap kecenderungan spesifik (misal: user yang pelit memberi nilai atau film yang selalu populer).
4. **Activation (Sigmoid)** :
    - Hasil dari operasi sebelumnya diproses menggunakan fungsi aktivasi Sigmoid untuk menghasilkan output dalam rentang 0 hingga 1.
#### Top-N Recommendation Result :
Setelah model dilatih, sistem dapat menghasilkan rekomendasi untuk pengguna tertentu. Berikut adalah contoh hasil rekomendasi untuk User ID 527:
<img width="253" height="262" alt="Inference" src="https://github.com/user-attachments/assets/494b2e39-1d80-4bf9-96d5-11bf0ee3ba8b" />

###### Interpretasi & Insight:
Film dengan Rating Tinggi dari User (Ground Truth):
User ini menyukai film-film klasik dan drama epik seperti:
- Babe (1995)
- Star Wars: Episode IV - A New Hope (1977)
- Aladdin (1992)
- Dances with Wolves (1990)
- Silence of the Lambs, The (1991)
#### Top 10 Rekomendasi dari Model:
Sistem berhasil merekomendasikan film yang memiliki kemiripan genre dan nuansa (Drama, Klasik, Cult) dengan preferensi user:
- Usual Suspects, The (1995)
- Shawshank Redemption, The (1994)
- Philadelphia Story, The (1940)
- Rear Window (1954)
- Reservoir Dogs (1992)
- Streetcar Named Desire, A (1951)
- Lawrence of Arabia (1962)
- Apocalypse Now (1979)
- Goodfellas (1990)
- Godfather: Part II, The (1974)
#### Analisis Kelebihan dan Kekurangan Pendekatan:
- **Kelebihan** : Model NCF mampu menangkap pola non-linear yang kompleks antara user dan item, memberikan akurasi yang lebih baik daripada faktorisasi matriks linier sederhana, dan tidak memerlukan fitur konten (metadata) yang lengkap.
- **Kekurangan** : Membutuhkan data rating yang cukup banyak (cold start problem untuk user baru) dan proses pelatihan (training) membutuhkan sumber daya komputasi yang lebih besar dibanding metode statistik.
### Evaluation
##### Metrik Evaluasi :
Metrik evaluasi yang digunakan untuk mengukur kinerja model dalam memprediksi rating adalah Root Mean Squared Error (RMSE).
###### Formula dan Cara Kerja :
RMSE mengukur rata-rata besarnya kesalahan prediksi model. Rumusnya adalah akar kuadrat dari rata-rata selisih kuadrat antara nilai prediksi dan nilai aktual.
$$RMSE = \sqrt{\frac{1}{N} \sum_{i=1}^{N} (y_{i} - \hat{y}_{i})^2}$$
**Keterangan** :
- $N$ : Jumlah sampel data.
- $y_{i}$ : Nilai rating sebenarnya (aktual)
- $\hat{y}_{i}$ : Nilai rating hasil prediksi model.
### Hasil Evaluasi
<img width="410" height="317" alt="Visualisasi matriks" src="https://github.com/user-attachments/assets/acef7c38-116d-434d-a8f9-3ad59855f600" />

###### Interpretasi & Insight:
- **Visualisasi Metrik Model** : Grafik di atas menunjukkan metrik root_mean_squared_error untuk data latih (train) dan data validasi (test) selama proses pelatihan. Ini adalah indikator kinerja model:
     - **Garis Biru (train)**: Menunjukkan root_mean_squared_error pada dataset pelatihan. Idealnya, nilai ini akan menurun seiring bertambahnya epoch, menandakan bahwa model belajar dari data.
     - **Garis Oranye (test)**: Menunjukkan val_root_mean_squared_error pada dataset validasi. Ini adalah metrik yang lebih penting untuk mengevaluasi kemampuan generalisasi model terhadap data yang belum pernah dilihat sebelumnya. Jika nilai ini stabil atau menurun seiring dengan data latih, itu menunjukkan model belajar dengan baik tanpa overfitting.
     
Dalam kasus ini, kedua garis menunjukkan penurunan awal yang signifikan, dan kemudian cenderung stabil. Ini menunjukkan bahwa model berhasil belajar dari data dan memiliki kemampuan generalisasi yang cukup baik.
- **Nilai RMSE** : Dari output training, kita bisa melihat nilai root_mean_squared_error (RMSE) terakhir pada akhir pelatihan. Nilai RMSE yang lebih rendah menunjukkan bahwa model memiliki prediksi yang lebih akurat. Berikut adalah nilai RMSE dari epoch terakhir (epoch 9):
    - **Train RMSE** : Sekitar 0.1973
    - **Validation RMSE** : Sekitar 0.2025
    
Nilai-nilai ini menunjukkan bahwa model memiliki kesalahan prediksi rata-rata yang relatif rendah, dan kinerja pada data validasi sangat dekat dengan kinerja pada data pelatihan, yang merupakan indikasi yang baik bahwa model tidak overfitting.
