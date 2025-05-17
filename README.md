# Laporan Proyek Machine Learning - Chalika Vanya Resya

## Domain Proyek
Industri *skincare* di Indonesia mengalami pertumbuhan yang sangat pesat dan saat ini menyumbang sekitar 30% dari total pangsa pasar industri kecantikan. Hal ini didorong oleh meningkatnya pendapatan masyarakat serta kesadaran akan pentingnya perawatan kulit sebagai bagian dari kesehatan tubuh secara menyeluruh. Seiring dengan semakin mudahnya akses terhadap produk melalui berbagai platform ritel seperti Watsons, Guardian, Sociolla, dan BeautyHaul, konsumen dihadapkan pada beragam pilihan produk yang sangat luas [[1]](https://www.researchinindonesia.com/insight/indonesia-skincare-market-overview).

Namun, di balik pertumbuhan pasar yang signifikan, masih banyak konsumen yang mengalami kesulitan dalam memilih produk *skincare* yang sesuai dengan kondisi dan kebutuhan kulit mereka. Kurangnya pemahaman mengenai tipe kulit, permasalahan yang dihadapi, serta kandungan dalam produk menjadi tantangan tersendiri. Pemilihan produk yang tidak tepat tidak hanya berisiko menimbulkan iritasi, tetapi juga membuat keputusan pembelian menjadi tidak efektif [[2]](https://doi.org/10.59188/jcs.v4i2.3026).

Dalam menjawab tantangan tersebut, sistem rekomendasi berbasis teknologi semakin banyak digunakan di industri kosmetik dan *skincare*, seperti menggunakan pendekatan **content-based filtering**, **collaborative filtering**, hingga model **deep learning** [[3]](https://onlinelibrary.wiley.com/doi/10.1111/jocd.16218). Dengan memanfaatkan pendekatan *machine learning*, sistem ini mampu memberikan saran produk yang lebih personal berdasarkan preferensi pengguna, riwayat interaksi, hingga kondisi kulit. Pendekatan ini tidak hanya meningkatkan pengalaman pengguna dalam memilih produk, tetapi juga membantu produsen memahami kebutuhan pasar secara lebih mendalam.

Maka dari itu, proyek ini berfokus pada pengembangan sistem rekomendasi produk *skincare* dan kosmetik yang mempertimbangkan tipe kulit pengguna serta kualitas produk berbasis data rating. Harapannya, sistem ini dapat membantu konsumen dalam mengambil keputusan yang lebih tepat dan efisien sesuai dengan karakteristik kulit masing-masing.
 
## Business Understanding

### Problem Statements
Berdasarkan latar belakang dan kebutuhan pengguna dalam menemukan produk *skincare* dan kosmetik yang sesuai, proyek ini dirancang untuk menjawab dua permasalahan utama:
1. Bagaimana cara membangun model yang dapat memberikan rekomendasi produk serupa dalam kategori yang sama dan sesuai dengan tipe kulit pengguna?
2. Bagaimana cara menghasilkan rekomendasi produk dalam kategori yang sama dan sesuai tipe kulit pengguna, yang juga mempertimbangkan produk dengan rating terbaik?

### Goals
Berdasarkan rumusan masalah yang dipaparkan, ditentukan beberapa tujuan dari proyek ini, yaitu:
1. Mengembangkan sistem rekomendasi yang mampu memberikan produk serupa berdasarkan kategori dan kecocokan dengan tipe kulit pengguna.
2. Menghasilkan rekomendasi produk serupa yang memiliki rating tinggi pengguna tidak hanya mendapatkan produk relevan tetapi juga berkualitas baik berdasarkan ulasan pengguna lain.

### Solution Statements
Untuk menyelesaikan permasalahan tersebut, proyek ini menggunakan dua algoritma *recommendation system* yang berbeda, yaitu:

1. **Content-Based Filtering**  
   - Menggunakan data preferensi pengguna untuk menghitung kemiripan antarproduk berdasarkan fitur seperti kategori dan tipe kulit.
   - Rekomendasi diberikan berdasarkan *similarity score* antara produk yang telah dipilih dengan produk lain yang memiliki karakteristik serupa.

2. **Clustering Recommendation System (K-Means)**  
   - Menggunakan algoritma *unsupervised learning* untuk mengelompokkan produk ke dalam beberapa klaster berdasarkan kesamaan fitur seperti rating, harga, dan jumlah ulasan.
   - Rekomendasi diberikan dengan menyarankan produk dari klaster yang sama dengan produk pilihan pengguna sehingga tetap relevan secara segmentasi.

### Evaluation Metrics
Untuk mengevaluasi performa kedua model sistem rekomendasi, digunakan metrik yang berbeda sesuai dengan pendekatan masing-masing:
- **Content-Based Filtering**:
  Dievaluasi menggunakan metrik **Precision@K** dan **Recall@K**:
  - **Precision@K**: mengukur proporsi produk yang relevan di antara hasil rekomendasi sebanyak K item teratas. Metrik ini menunjukkan **seberapa akurat** rekomendasi yang diberikan.
  - **Recall@K**: mengukur sejauh mana sistem berhasil menemukan semua produk relevan dalam K rekomendasi. Metrik ini menggambarkan **cakupan sistem terhadap item relevan**.
- **Clustering-Based Recommendation (K-Means)**:
  Dievaluasi menggunakan metrik **Silhouette Score**:
  - **Silhouette Score**: menilai kualitas klasterisasi dengan membandingkan kedekatan data dalam klaster yang sama dan perbedaannya dengan klaster lain. Nilai mendekati 1 menunjukkan pemisahan klaster yang baik.

## Data Understanding
*Dataset* yang digunakan dalam proyek ini merupakan **data sintetis** yang merepresentasikan informasi produk-produk kecantikan. Setiap baris dalam data menggambarkan satu produk.

Data berasal dari platform Kaggle dan dapat diakses melalui tautan [Beauty & Cosmetics Products Dataset](https://www.kaggle.com/datasets/waqi786/most-used-beauty-cosmetics-products-in-the-world). *Dataset* ini berupa file `.csv` dengan ukuran 1.65 MB dan bernama `most_used_beauty_cosmetics_products_extended.csv`. Dataset mencakup total 15.000 baris data 14 fitur awal sebelum dilakukan *feature extraction*. 

### 🧾 Deskripsi Variabel

| No | Kolom               | Tipe Data | Deskripsi                                                             |
| -- | ------------------- | --------- | --------------------------------------------------------------------- |
| 1  | `Product_Name`      | object    | Nama dari produk kecantikan                                           |
| 2  | `Brand`             | object    | Merek atau produsen dari produk kecantikan                            |
| 3  | `Category`          | object    | Kategori produk (seperti moisturizer, cleanser, face mask, serum)     |
| 4  | `Usage_Frequency`   | object    | Frekuensi penggunaan produk (daily, weekly, occasional)               |
| 5  | `Price_USD`         | float64   | Harga produk dalam satuan USD                                         |
| 6  | `Rating`            | float64   | Nilai kepuasan pengguna (skala 1–5)                                   |
| 7  | `Number_of_Reviews` | int64     | Jumlah ulasan pengguna, mengindikasikan popularitas produk            |
| 8  | `Product_Size`      | object    | Ukuran atau volume produk dalam ml                                    |
| 9  | `Skin_Type`         | object    | Jenis kulit target (oily, dry, normal, combination, sensitive)        |
| 10 | `Gender_Target`     | object    | Segmentasi target pengguna berdasarkan gender (male, female, unisex)  |
| 11 | `Packaging_Type`    | object    | Jenis kemasan (bottle, tube, jar, compact)                            |
| 12 | `Main_Ingredient`   | object    | Bahan utama atau kandungan aktif (misalnya Retinol, Vitamin C)        |
| 13 | `Cruelty_Free`      | bool      | Apakah produk tidak diuji pada hewan (True/False)                     |
| 14 | `Country_of_Origin` | object    | Negara asal atau produksi (misalnya USA, South Korea)                 |

---

### 📈 Statistik Deskriptif (Variabel Numerik)

| Statistik | `Price_USD` | `Rating` | `Number_of_Reviews` |
| --------- | ----------- | -------- | ------------------- |
| Count     | 15000       | 15000    | 15000               |
| Mean      | 80.13       | 3.00     | 5014.23             |
| Std       | 40.40       | 1.17     | 2855.67             |
| Min       | 10.00       | 1.00     | 52                  |
| 25%       | 45.48       | 2.00     | 2562.00             |
| 50%       | 80.04       | 3.00     | 5002.00             |
| 75%       | 114.76      | 4.00     | 7497.00             |
| Max       | 149.99      | 5.00     | 10000.00            |

---

### 🧮 Statistik Ringkasan (Variabel Kategorikal)

| Kolom               | Count | Unique | Nilai Terbanyak (Top) | Frekuensi |
| ------------------- | ----- | ------ | --------------------- | --------- |
| `Product_Name`      | 15000 | 120    | Super Setting Spray   | 154       |
| `Brand`             | 15000 | 40     | Milk Makeup           | 426       |
| `Category`          | 15000 | 24     | Serum                 | 710       |
| `Usage_Frequency`   | 15000 | 4      | Occasional            | 3794      |
| `Product_Size`      | 15000 | 6      | 100ml                 | 2551      |
| `Skin_Type`         | 15000 | 5      | Combination           | 3060      |
| `Gender_Target`     | 15000 | 3      | Male                  | 5017      |
| `Packaging_Type`    | 15000 | 6      | Jar                   | 2567      |
| `Main_Ingredient`   | 15000 | 7      | Retinol               | 2180      |
| `Country_of_Origin` | 15000 | 8      | Italy                 | 1942      |

---

### Exploratory Data Analysis (EDA)
Untuk memahami karakteristik data secara menyeluruh, dilakukan eksplorasi awal yang mencakup:

1. **Pemeriksaan Struktur Dataset**  
   Meliputi jumlah baris dan kolom, tipe data, serta identifikasi nilai null (*missing values*). Setelah diperiksa, tidak terdapat *missing values* pada *dataset* sehingga tidak perlu ada penanganan.

2. **Deteksi Outliers**  
   Visualisasi boxplot digunakan untuk melihat kemunculan nilai ekstrem pada kolom numerik.
   
   ![outliers](https://github.com/user-attachments/assets/d6a39db4-a74c-4aee-a1a6-261572849fe4)

   Visualisasi di atas menunjukkan bahwa *dataset* tidak memiliki *outliers* sehingga tidak diperlukan penanganan apapun.

4. **Distribusi data numerik**  
   Pemeriksaan distribusi setiap variabel numerik menggunakan histogram.
   
   ![histogram](https://github.com/user-attachments/assets/d74e4bbc-8f38-4b42-804a-ce494c0d5bf0)
   
   Visualisasi histogram tersebut menampilkan distribusi data numerik yang **cukup seragam** (*uniform*). Hal ini menunjukkan bahwa tidak ada data yang mendominasi di satu sisi (nilai ekstrem/*outliers*).

6. **Distribusi data kategorikal**  
   Pemeriksaan distribusi setiap variabel numerik menggunakan countplot dengan pembatasan nilai sebanyak 10 untuk tujuan keterbacaan.
   
   ![countplot](https://github.com/user-attachments/assets/e61ee7f1-3823-4bc5-97d4-0e23cfd43ee5)

   Terlihat distribusi seluruh fitur cukup merata (*uniform*) dengan tidak ada nilai unik yang terlalu dominan. Hal ini mengindikasikan bahwa data tidak mengalami ketidakseimbangan (*imbalance*) yang berlebihan sehingga tidak diperlukan penanganan khusus seperti *resampling*.

## Data Preparation

Tahapan persiapan data mencakup penyesuaian nama fitur, konversi tipe data, pembersihan *outliers*, ekstraksi fitur, hingga pembangunan *preprocessing pipeline*.

### 1. **Perubahan nama fitur**
Hal yang dilakukan mencakup mengubah penulisan menjadi *lowercase* untuk kemudahan pemanggilan.
```py
df.columns = df.columns.str.lower()
```
### 2. **Konversi tipe data**
Mengubah tipe data kolom `cruelty_free` dari `bool` menjadi `int` sebagai bentuk *encoding* awal.
```py
df['cruelty_free'] = df['cruelty_free'].astype(int)
```

### 3. **Missing Values**
Tidak terdapat *missing values* pada *dataset* sehingga tidak perlu ada penanganan.
```py
df.isnull().sum()
```

### 4. **Typos/Inconsistencies**
Dilakukan pemeriksaan terhadap kesalahan penulisan (typo) pada setiap nilai unik data kategorikal.
```py
# store categorical columns
cat_cols = df.select_dtypes('object').columns.tolist()

# check for typos or inconsistencies
for feat in cat_cols:
  unique_values = df[feat].sort_values().unique() # sort the unique values alphabetically
  print(f'Unique values of {feat}:')
```
Pemeriksaan menunjukkan tidak terdapat kesalahan penulisan pada setiap nilai. Ditemukan juga informasi bahwa nama produk memuat nama kategori produk, yang memungkinkan pemeriksaan terhadap inkonsistensi data; apakah setiap produk sudah termasuk pada kategori yang sesuai.

Berikut adalah beberapa nilai unik pada kolom `product_name` dan `category`:
```py
Unique values of product_name:
['Divine BB Cream' 'Divine Blush' 'Divine Bronzer' 'Divine CC Cream'
 'Divine Cleanser' 'Divine Concealer' 'Divine Contour' 'Divine Exfoliator'
 'Divine Eye Shadow' 'Divine Eyeliner' 'Divine Face Mask'
 'Divine Face Oil' 'Divine Foundation' 'Divine Highlighter'
 'Divine Lip Gloss' 'Divine Lip Liner' 'Divine Lipstick'
 'Divine Makeup Remover' 'Divine Mascara' 'Divine Moisturizer'
 'Divine Powder' 'Divine Primer' 'Divine Serum' 'Divine Setting Spray'
... ]
```
```py
Unique values of category:
['BB Cream' 'Blush' 'Bronzer' 'CC Cream' 'Cleanser' 'Concealer' 'Contour'
 'Exfoliator' 'Eye Shadow' 'Eyeliner' 'Face Mask' 'Face Oil' 'Foundation'
 'Highlighter' 'Lip Gloss' 'Lip Liner' 'Lipstick' 'Makeup Remover'
 'Mascara' 'Moisturizer' 'Powder' 'Primer' 'Serum' 'Setting Spray']
```

Pemeriksaan inkonsistensi data dilakukan dengan mengambil salah satu produk dan menampilkan informasi kategori dan brand pada produk tersebut. Untuk mengatasi permasalahan ini, dilakukan ekstraksi kategori berdasarkan
nama produk tersebut.

```py
# function to fix inconsistencies by extracting the category from product name
def extract_category(product_name, categories):
  name = product_name.lower()

  for category in categories:
    if category in name:
      return category.title() # convert to title case
  return 'Unknown'

# apply fix
categories = df['category'].str.lower().unique()
df['category'] = df['product_name'].apply(lambda x: extract_category(x, categories))
```

### 5. **Duplicated Data**
Tidak terdapat data duplikat pada *dataset* sehingga tidak perlu ada penanganan.
```py
df.duplicated().sum()
```

### 6. **Feature Extraction**
Rekayasa fitur dilakukan untuk menambah informasi yang dapat membantu proses pemodelan melalui fitur yang sudah ada.
```py
# extract the numeric size and remove the unit
df['product_size_(ml)'] = df['product_size'].str.extract(r'(\d+)').astype(int)

# remove column product_size
df.drop('product_size', axis=1, inplace=True)
```

```py
# combine the product name and the brand to create a unique product id
df['unique_product_id'] = df['product_name'] + '_' + df['brand']
```

Fitur-fitur baru yang dihasilkan antara lain:
- `product_size_(ml)`: ukuran netto produk dalam satuan ml
- `unique_product_id`: id unik produk dengan mengambil informasi nama dan *brand* produk

### 7. **Text Normalization**
Normalisasi teks dilakukan untuk memastikan keseragaman format teks dan menghindari duplikasi/inkonsistensi sebelum masuk ke tahap pemodelan, meliputi:
- Mengubah seluruh huruf menjadi ***lowercase***
- **Menghapus *whitespace***
- **Menghilangkan tanda baca** dan karakter non-alfabet lainnya

```py
# text normalization
df_clean = df.copy()

cat_cols = df_clean.select_dtypes('object').columns # redefine categorical columns
for col in cat_cols:
  df_clean[col] = df_clean[col].str.strip().str.lower() # remove whitespaces and convert to lowercase

  # remove punctuations except for the underscore in unique_product_id
  if col != 'unique_product_id':
    df_clean[col] = df_clean[col].str.replace(r'[^\w\s_]', '', regex=True)
  else:
    df_clean[col] = df_clean[col].str.replace(r'[^\w\s]', '', regex=True)
```
Karakter *underscore* (_) pada kolom `unique_product_id` tidak dihilangkan untuk menjaga perannya sebagai *identifier* unik.

```py
# clean categories preserve full phrase
df_clean['category_clean'] = df_clean['category'].str.replace(' ', '_') # clean categories preserve full phrase
```
Penulisan setiap kategori juga diperbaiki dengan mengganti spasi menjadi *underscore* (_) untuk mempertahankan setiap kata secara utuh sebelum proses *text embedding*. Hal ini dilakukan agar kategori dengan kata lebih dari satu dibaca terpisah.

### 8. **Drop duplicates - By Product Name**
Sebelum melakukan perhitungan *cosine similarity* dan *modeling*, langkah awal yang dilakukan adalah memastikan bahwa tidak terdapat duplikat `product_name` dalam data. Hal ini penting karena *cosine similarity* menghitung kemiripan antarproduk secara berpasangan. Adanya data nama produk yang duplikat dapat menyebabkan rekomendasi yang redundan atau bias, matriks kemiripan yang tidak akurat, hingga kesalahan dalam pemetaan indeks produk.
```py
# drop duplicates based on product_name
unique_prod_df = df_clean.drop_duplicates(subset='unique_product_id', keep='first').reset_index(drop=True)
```

### 9. **Preprocessing Pipeline**
*Pipeline* ini hanya digunakan untuk menyiapkan data sebelum *modeling* menggunakan algoritma **KMeans Clustering**, meliputi:
-   **Standarisasi** fitur numerik
-   **Encoding** fitur kategorikal menggunakan One-Hot Encoding.

```py
# feature selection
num_cols = ['rating', 'number_of_reviews', 'price_usd']
cat_cols = ['category', 'main_ingredient', 'skin_type']

# preprocessing pipeline
preprocessor = ColumnTransformer([
    ('num', StandardScaler(), num_cols),
    ('cat', OneHotEncoder(handle_unknown='ignore'), cat_cols)
])

# apply preprocessing 
X_cluster = preprocessor.fit_transform(df_cluster)
```
Sebagai catatan, `df_cluster` adalah dataframe yang digunakan untuk pemodelan **KMeans Clustering**.

## Modeling & Evaluation
Setelah data siap digunakan, dilakukan pelatihan model menggunakan dua algoritma dengan karakteristik berbeda: **Content-Based Filtering** dan **Clustering-Based Recommendation System**. Kedua model ini dipilih karena mewakili algoritma penentuan rekomendasi produk yang berbeda, satu melalui preferensi pengguna, sedangkan yang lainnya berdasarkan hasil klasterisasi berdasarkan kesamaan karakteristik produk yang diwakilkan oleh fitur.
- **Content-Based Filtering** [[4]](https://www.ibm.com/think/topics/content-based-filtering):
  - Cara kerja: metode sistem rekomendasi yang menyarankan produk berdasarkan kemiripan fitur dengan item yang sebelumnya disukai pengguna.
  - Kelebihan: unggul dalam menangani *cold start* untuk item baru karena tidak bergantung pada interaksi historis pengguna dan memberikan transparansi yang tinggi karena fitur-fitur yang digunakan dapat menjelaskan alasan rekomendasi secara eksplisit.
  - Kekurangan: keterbatasan fitur (jika representasi data tidak cukup menggambarkan preferensi pengguna, rekomendasi bisa kurang akurat) serta model cenderung menghasilkan rekomendasi yang terlalu seragam atau mirip dengan produk sebelumnya (*overspecialization*) sehingga membatasi eksplorasi produk baru.
- **Clustering-Based Recommendation System** [[5]](https://www.geeksforgeeks.org/clustering-based-algorithms-in-recommendation-system/):
  - Cara kerja: menggunakan algoritma klasterisasi (seperti KMeans) untuk mengelompokkan produk ke dalam segmen berdasarkan kemiripan karakteristik, seperti kategori, rating, dan tipe kulit yang ditargetkan.
  - Kelebihan: menghasilkan segmentasi produk yang bermakna (berdasarkan kesamaan beberapa karakteristik) sehingga memungkinkan sistem memberikan rekomendasi yang lebih terpersonalisasi.
  - Kekurangan: performa model sangat bergantung pada kualitas fitur yang digunakan serta efektivitas metode klasterisasi yang diterapkan (berpotensi menghasilkan rekomendasi yang kurang relevan apabila segmentasi kurang optimal).

### Content-Based Filtering 
Model rekomendasi pertama menggunakan pendekatan ***content-based filtering*** yang memanfaatkan informasi kategori produk dan tipe kulit pengguna. Model ini menerima input berupa nama produk, nama brand, dan tipe kulit. Tahapan berikutnya adalah:
- *Filtering* terhadap produk berdasarkan kategori yang sama dengan produk input
- *Filtering* berdasarkan tipe kulit user
- Perhitungan skor kemiripan (*similarity score*) untuk memberikan rekomendasi top-N produk yang paling relevan.

#### TF-IDF Vectorizer
Untuk merepresentasikan fitur dari setiap kategori produk, digunakan pendekatan **TF-IDF Vectorizer**. Teknik ini menghasilkan sebuah matriks yang mencerminkan pentingnya sebuah kata (fitur) dalam setiap kategori yang kemudian digunakan untuk menghitung kemiripan antarproduk berdasarkan kategorinya.

Contoh hasil *tf-idf table*:

| unique_product_id                      | concealer | serum | face_oil | bb_cream | eye_shadow | bronzer | blush | face_mask | cc_cream | lipstick | ... | moisturizer | foundation | contour | setting_spray | lip_liner | makeup_remover | lip_gloss | powder | cleanser | mascara |
|----------------------------------------|-----------|-------|----------|----------|-------------|---------|--------|------------|-----------|-----------|-----|--------------|-------------|----------|----------------|------------|------------------|------------|---------|-----------|---------|
| magic eyeliner_hourglass               | 0.0       | 0.0   | 0.0      | 0.0      | 0.0         | 0.0     | 0.0    | 0.0        | 0.0       | 0.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| magic contour_rare beauty              | 0.0       | 0.0   | 0.0      | 0.0      | 0.0         | 0.0     | 0.0    | 0.0        | 0.0       | 0.0       | ... | 0.0          | 0.0         | 1.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| perfect lipstick_glossier              | 0.0       | 0.0   | 0.0      | 0.0      | 0.0         | 0.0     | 0.0    | 0.0        | 0.0       | 1.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| divine lipstick_huda beauty            | 0.0       | 0.0   | 0.0      | 0.0      | 0.0         | 0.0     | 0.0    | 0.0        | 0.0       | 1.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| super cc cream_bite beauty             | 0.0       | 0.0   | 0.0      | 0.0      | 0.0         | 0.0     | 0.0    | 0.0        | 1.0       | 0.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| perfect lipstick_rms beauty            | 0.0       | 0.0   | 0.0      | 0.0      | 0.0         | 0.0     | 0.0    | 0.0        | 0.0       | 1.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| ultra bb cream_hourglass               | 0.0       | 0.0   | 0.0      | 1.0      | 0.0         | 0.0     | 0.0    | 0.0        | 0.0       | 0.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| magic cc cream_natasha denona         | 0.0       | 0.0   | 0.0      | 0.0      | 0.0         | 0.0     | 0.0    | 0.0        | 1.0       | 0.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| perfect bb cream_too faced             | 0.0       | 0.0   | 0.0      | 1.0      | 0.0         | 0.0     | 0.0    | 0.0        | 0.0       | 0.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 0.0     | 0.0       | 0.0     |
| divine powder_kylie cosmetics          | 0.0       | 0.0   | 0.0      | 0.0      | 0.0         | 0.0     | 0.0    | 0.0        | 0.0       | 0.0       | ... | 0.0          | 0.0         | 0.0      | 0.0            | 0.0        | 0.0              | 0.0        | 1.0     | 0.0       | 0.0     |

Sebagai contoh, produk 'divine lip gloss' memiliki skor korelasi sebesar 1 terhadap kategori `lip gloss`, yang menandakan representasi fitur TF-IDF bekerja sebagaimana mestinya.

#### Cosine Similarity
Proses selanjutnya adalah menghitung derajat kemiripan antarproduk berdasarkan representasi nama produk yang telah di-*vectorize*. Hasil dari proses ini akan digunakan untuk mencari kandidat produk yang layak direkomendasikan berdasarkan skor kemiripannya terhadap input produk (*anchor product*).

Contoh hasil *cosine similarity table*:

| unique_product_id                          | ultra serum_kylie cosmetics | ultra powder_elf | perfect powder_juvias place | ultra eye shadow_bobby brown | magic face oil_nars | perfect lip liner_becca | super face oil_clinique | divine lipstick_clinique | magic face mask_natasha denona | perfect primer_rms beauty | ultra face mask_colourpop | super face oil_milk makeup | super lip gloss_perricone md | super moisturizer_tarte | divine primer_farsali |
|--------------------------------------------|------------------------------|-------------------|-----------------------------|------------------------------|----------------------|--------------------------|---------------------------|----------------------------|-------------------------------|----------------------------|---------------------------|-----------------------------|----------------------------|-----------------------|------------------------|
| perfect concealer_kiehls                   | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| perfect eyeliner_it cosmetics              | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| magic contour_tatcha                       | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| ultra concealer_kvd beauty                 | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| perfect serum_kvd beauty                   | 1.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| magic foundation_bobby brown              | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| divine makeup remover_urban decay         | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| divine eye shadow_anastasia beverly hills | 0.0                          | 0.0               | 0.0                         | 1.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| perfect face mask_becca                   | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 1.0                           | 0.0                        | 1.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| super cc cream_colourpop                  | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| ultra serum_drunk elephant                | 1.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| perfect serum_patrick ta                 | 1.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| perfect bronzer_pat mcgrath labs         | 0.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| ultra powder_kvd beauty                  | 0.0                          | 1.0               | 1.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |
| divine serum_yves saint laurent          | 1.0                          | 0.0               | 0.0                         | 0.0                          | 0.0                  | 0.0                      | 0.0                       | 0.0                        | 0.0                           | 0.0                        | 0.0                       | 0.0                         | 0.0                        | 0.0                   | 0.0                    |

Terlihat bahwa produk-produk dalam kategori yang sama akan memiliki skor kemiripan tertinggi (1.0), seperti pada kasus `magic serum_urban decay` dan `divine serum_too faced` yang keduanya berada dalam kategori serum.

#### Model Development
Fungsi `recommend_similar_product()` menerima input berupa nama produk, *brand*, dan tipe kulit pengguna. Alur fungsi ini adalah sebagai berikut:
1. Menggabungkan nama produk dan brand untuk mencari produk acuan (*anchor product*)
2. Melakukan *filtering* terhadap produk lain berdasarkan kategori yang sama
3. Menyaring kembali data hasil *filtering* sebelumnya berdasarkan kecocokan tipe kulit
4. Mengurutkan hasil berdasarkan skor kemiripan
5. Mengembalikan top-N produk teratas sebagai rekomendasi.

Jika tidak ditemukan produk lain dalam kategori tersebut yang sesuai dengan tipe kulit pengguna, model tetap memberikan rekomendasi dari kategori yang sama tanpa mempertimbangkan kecocokan tipe kulit.

#### Contoh Hasil Prediksi
Berikut adalah contoh input dan beberapa output yang dihasilkan model (dengan 10 rekomendasi produk):
```py
Enter a product name: divine moisturizer
From which brand?: kiehls
What is your skin type (e.g., sensitive, dry, normal, oily, combination): oily

Recommended products for skin type 'Oily' (Category: Moisturizer):

1. Divine Moisturizer by Tatcha
    Price: $114.12
    Package: 250ml in tube
    Main Ingredient: shea butter
    Most suitable for: oily skin
    Rating: 4.8

2. Super Moisturizer by Kylie Cosmetics
    Price: $129.16
    Package: 250ml in spray
    Main Ingredient: salicylic acid
    Most suitable for: oily skin
    Rating: 4.6

3. Ultra Moisturizer by Bobby Brown
    Price: $85.92
    Package: 200ml in tube
    Main Ingredient: salicylic acid
    Most suitable for: oily skin
    Rating: 2.8

4. Ultra Moisturizer by Drunk Elephant
    Price: $35.33
    Package: 100ml in stick
    Main Ingredient: shea butter
    Most suitable for: oily skin
    Rating: 5.0
...

Additional info:
Similarity scores retrieved: 4571
Filtered by skin type + category: 36
Filtered similarity matches: 10
```

#### Model Evaluation
Fungsi `evaluate_model()` digunakan untuk mengevaluasi performa sistem rekomendasi menggunakan dua metrik utama, yaitu:
- **Precision@K**: Mengukur proporsi item yang relevan di antara *K* produk yang direkomendasikan.
```math
\text{Precision@K} = \frac{\text{Jumlah item relevan di top-K rekomendasi}}{K}
```
- **Recall@K**: Mengukur proporsi item relevan yang berhasil direkomendasikan dari seluruh item relevan yang tersedia untuk pengguna.
```math
\text{Recall@K} = \frac{\text{Jumlah item relevan di top-K rekomendasi}}{\text{Total item relevan yang tersedia untuk user tersebut}}
```

Kedua metrik ini digunakan untuk menilai seberapa **akurat** (precision) dan **menyeluruh** (recall) hasil rekomendasi model terhadap kebutuhan pengguna.

Berdasarkan contoh input dan output sebelumnya, hasil evaluasi metrik adalah sebagai berikut:
- **Precision@K = 1.0**  
  Nilai ini mengindikasikan seluruh produk yang direkomendasikan merupakan produk yang **relevan** dengan kebutuhan user (termasuk dalam kategori dan tipe kulit yang sesuai). Hal Ini menunjukkan bahwa sistem cukup akurat dalam memilih produk relevan dari total yang direkomendasikan.
- **Recall@K = 0.28**  
  Nilai recall yang cukup rendah menunjukkan bahwa **jumlah produk relevan yang berhasil ditampilkan** oleh sistem masih terbatas. Dalam kasus ini, sistem hanya berhasil menampilkan **10 dari total 36** produk yang dianggap relevan dalam dataset (dengan mempertimbangkan filter kategori dan tipe kulit).


### Clustering-Based Algorithm
Model rekomendasi kedua menggunakan pendekatan **clustering-based recommendation** dengan algoritma *unsupervised learning* **K-Means**. Algoritma ini mengelompokkan produk berdasarkan kemiripan beberapa fitur, seperti:
- `price_usd`
- `rating`
- `number_of_reviews`
- `category`
- `skin_type`
- `main_ingredient`
melalui perhitungan seperti **Euclidean distance**.

Rekomendasi diberikan dari produk yang berada di **cluster yang sama** dengan kategori input, dengan penyesuaian lebih lanjut melalui *filtering* berdasarkan *kategori* dan *skin type* untuk menghasilkan hasil yang lebih personal dan relevan dengan kebutuhan pengguna.

Pendekatan *clustering* digunakan karena terkadang lebih efektif dalam mengelompokkan produk atau pengguna dengan karakteristik yang mirip, terutama saat data yang tersedia tidak lengkap atau terbatas. Dibandingkan metode yang mengandalkan kemiripan antar item atau pengguna, *clustering* cenderung lebih ringan secara komputasi dan tetap bisa memberikan rekomendasi yang relevan meskipun informasi interaksi pengguna terbatas [[6]](https://www.researchgate.net/publication/354883495_Review_of_Clustering-Based_Recommender_Systems).

#### Clustering
Langkah awal yang dilakukan adalah menentukan jumlah kluster optimal menggunakan **metode elbow** dan perhitungan ***silhouette score***. Berikut ringkasan analisis penentuan jumlah klaster dari kedua pendekatan tersebut serta dari data statistik klaster yang dibentuk:

- **Cluster = 2**  
  - *Silhouette Score* tertinggi, tetapi hasil terlalu generik.
  - Hanya membedakan dua kelompok besar: produk **affordable** vs. **premium** dengan persebaran rating yang kurang jelas.
  
- **Cluster = 8**  
  - Memberikan segmentasi yang lebih detail, tetapi skor *silhouette* sangat rendah.
  - Berisiko menghasilkan *cluster* yang tumpang tindih atau tidak bermakna (*noisy*).

- **Cluster = 6**  
  - **Dipilih sebagai solusi** karena berada diantara keduanya.
  - Jumlah *cluster* tidak terlalu sedikit maupun terlalu banyak.
  - *Silhouette score* masih cukup baik untuk interpretasi dan segmentasi yang masuk akal.
 
```py
# build model using 6 as number of clusters
kmeans = KMeans(n_clusters=6, random_state=42)
labels = kmeans.fit_predict(X_cluster)
df_cluster['cluster'] = labels + 1 # labeling starts from 1
```

| cluster | rating   | number_of_reviews | price_usd  |
|---------|----------|-------------------|------------|
| 1       | 1.876623 | 7453.212987       | 55.785130  |
| 2       | 1.806010 | 3476.594629       | 113.945396 |
| 3       | 2.850000 | 2235.188107       | 40.166760  |
| 4       | 3.449452 | 7781.843836       | 119.804055 |
| 5       | 4.050501 | 2534.266094       | 109.810844 |
| 6       | 4.135724 | 6772.993481       | 47.429557  |

Berikut adalah beberapa interpretasi segmentasi dari data statistik setiap kluster (dengan k=6):
- **Cluster 6**: Produk *affordable* dengan rating tinggi (contoh: rating 4.14, harga sekitar \$47)
- **Cluster 5**: Produk premium dengan rating tinggi (contoh: rating 4.05, harga sekitar \$110)
- **Cluster 2**: Produk mahal dengan rating rendah
- **Cluster 1**: Produk populer namun memiliki rating rendah

Segmentasi ini memberikan **klasterisasi *price-performance*** yang lebih baik dan distribusi produk yang seimbang sehingga dapat menghasilkan rekomendasi yang lebih kontekstual.

#### Model Development
Fungsi `recommend_by_cluster()` bertujuan untuk merekomendasikan produk berdasarkan hasil segmentasi *cluster* yang telah dibentuk. Alur kerjanya adalah sebagai berikut:
1. Menerima input berupa **kategori produk** dan **tipe kulit pengguna**.
2. Melakukan **filtering** data sesuai dengan kategori dan tipe kulit tersebut.
3. Mengidentifikasi **cluster terbanyak** dari hasil filter (*cluster* dominan).
4. Menyaring data lebih lanjut hanya pada produk yang berada di **cluster tersebut**.
5. Mengurutkan hasil berdasarkan **rating tertinggi** dan **jumlah review terbanyak**.
6. Mengembalikan rekomendasi sejumlah `top-n` produk yang paling relevan.

#### Contoh Hasil Prediksi
Berikut adalah contoh input dan beberapa output yang dihasilkan model (dengan 5 rekomendasi produk):
```py
Enter product category: face mask
What is your skin type (e.g., sensitive, dry, normal, oily, combination): sensitive

Top 5 'Face Mask' products for 'Sensitive' skin (Cluster 5):

1. Divine Face Mask by Milk Makeup
    Category: Face Mask, Rating: 4.8, Reviews: 5595
    Skin Type: Sensitive, Price: $112.12
    Cluster: 5

2. Ultra Face Mask by Becca
    Category: Face Mask, Rating: 4.8, Reviews: 3765
    Skin Type: Sensitive, Price: $102.98
    Cluster: 5

3. Magic Face Mask by Make Up For Ever
    Category: Face Mask, Rating: 4.8, Reviews: 1078
    Skin Type: Sensitive, Price: $62.29
    Cluster: 5

4. Super Face Mask by Anastasia Beverly Hills
    Category: Face Mask, Rating: 4.7, Reviews: 3264
    Skin Type: Sensitive, Price: $145.69
    Cluster: 5

5. Ultra Face Mask by Perricone Md
    Category: Face Mask, Rating: 4.7, Reviews: 1028
    Skin Type: Sensitive, Price: $61.47
    Cluster: 5
```
#### Model Evaluation
```py
# silhouette score
sil_score = silhouette_score(X_cluster, labels)
```

Model menggunakan **silhouette score** sebagai metrik evaluasi kualitas *cluster*. Untuk konfigurasi `k = 6`, diperoleh nilai *silhouette* score sebesar **0.1089**.
Walaupun terlihat kecil, hal ini masih dapat dimaklumi mengingat beberapa faktor yang mungkin memengaruhi rendahnya nilai *silhouette score*, seperti:
- **Dimensi data tinggi** (terutama akibat *encoding* fitur kategorikal menggunakan OneHotEncoder)
- ***Overlap* antarsegmen produk** (misalnya, produk yang sama cocok untuk beberapa tipe kulit atau kategori)
- **Noise pada data sintetis atau kurang representatifnya distribusi produk**
- **Distribusi harga dan rating yang tidak merata atau kurang jelas**
- dll.

Dengan mempertimbangkan hal-hal tersebut, pemilihan `k = 6` tetap memberikan segmentasi yang cukup *interpretable* dan mendukung personalisasi rekomendasi yang terbilang baik.

## Final Conclusion

Proyek ini bertujuan untuk mengembangkan sistem rekomendasi produk skincare dan kosmetik yang mampu memberikan saran personalisasi berdasarkan karakteristik produk dan kebutuhan pengguna, khususnya tipe kulit. Dua pendekatan utama yang digunakan adalah **Content-Based Filtering** dan **Clustering-Based Recommendation (K-Means)**. Evaluasi dilakukan menggunakan metrik seperti *precision*, *recall*, dan *silhouette score*, dengan hasil yang menunjukkan bahwa kedua metode memiliki kekuatan masing-masing dalam menjawab permasalahan bisnis yang diajukan.

Pendekatan **Content-Based Filtering** menunjukkan performa yang sangat baik dari sisi **precision**, yang berarti produk yang direkomendasikan umumnya relevan dengan preferensi pengguna. Hal ini membuktikan bahwa sistem berhasil mengenali kebutuhan pengguna berdasarkan fitur seperti kategori produk dan tipe kulit. Namun, **nilai recall yang masih rendah** mengindikasikan bahwa sistem belum mampu menampilkan seluruh produk relevan secara menyeluruh. Untuk meningkatkan performa ini, sistem dapat disempurnakan melalui peningkatan jumlah hasil rekomendasi (*top\_n*), penambahan kompleksitas dalam representasi teks, serta penyempurnaan pada tahap filtering data.

Sementara itu, pendekatan **Clustering-Based Recommendation menggunakan K-Means** menunjukkan **silhouette score yang relatif rendah** (0.1089), yang berarti bahwa kualitas pemisahan antarklaster belum optimal. Namun, hasil segmentasi masih cukup **interpretable dan berguna** untuk mengelompokkan produk ke dalam segmen yang berbeda. Hal ini membuka potensi untuk menyusun rekomendasi berbasis klaster yang bisa ditargetkan untuk kelompok pengguna tertentu. Perbaikan yang dapat dilakukan antara lain dengan menggunakan algoritma *clustering* alternatif seperti **DBSCAN** yang lebih baik dalam menangani *noise* dan segmentasi yang tumpang tindih, serta menerapkan **dimensionality reduction** seperti PCA sebelum proses *clustering* untuk meningkatkan separabilitas antarklaster.

Secara keseluruhan, kedua pendekatan telah mampu memberikan rekomendasi produk yang sesuai dengan kebutuhan pengguna dan secara langsung mendukung tujuan bisnis dalam membantu pengguna menemukan produk skincare dan kosmetik yang relevan. Model-model ini tidak hanya menjawab *problem statement* yang diajukan, tetapi juga berkontribusi nyata dalam menciptakan pengalaman pencarian produk yang lebih personal, terarah, dan efisien. Untuk kedepannya, sistem ini masih dapat dikembangkan lebih lanjut dengan eksperimen lanjutan terhadap metode representasi data dan algoritma pembelajaran mesin yang lebih kompleks guna meningkatkan kualitas dan akurasi rekomendasi.

## References
[1] Indonesia, “Indonesian skincare market overview: Trends and insights,” Indonesian Skincare Market: Trends and Insights, https://www.researchinindonesia.com/insight/indonesia-skincare-market-overview (accessed May 17, 2025). 

[2] A. Wildana Al Kautsar and H. Hasrullah, “Sistem Rekomendasi pemilihan produk skincare,” Journal of Comprehensive Science (JCS), vol. 4, no. 2, pp. 534–543, Feb. 2025. doi:10.59188/jcs.v4i2.3026 

[3] J. Lee et al., “Deep learning‐based Skin Care Product Recommendation: A focus on cosmetic ingredient analysis and facial skin conditions,” Journal of Cosmetic Dermatology, vol. 23, no. 6, pp. 2066–2077, Feb. 2024. doi:10.1111/jocd.16218 

[4] “What is content-based filtering?,” IBM, https://www.ibm.com/think/topics/content-based-filtering (accessed May 17, 2025). 

[5] “Clustering based algorithms in recommendation system,” GeeksforGeeks, https://www.geeksforgeeks.org/clustering-based-algorithms-in-recommendation-system/ (accessed May 17, 2025). 

[6] B. Irina and M. Koroteev, “Review of Clustering-Based Recommender Systems,” Sep. 2021. [doi:10.59188/jcs.v4i2.3026 ](http://dx.doi.org/10.13140/RG.2.2.34745.90725)
