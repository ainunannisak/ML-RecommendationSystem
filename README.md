# ML-RecommendationSystem

## Project Overview
Perpustakaan digital dan toko buku online semakin banyak dan mudah untuk diakses. Ini memberikan pembaca keleluasaan untuk memilih, namun juga memberikan tantangan untuk menemukan buku yang sesuai dengan minat dan preferensi mereka. Untuk mengatasi situasi ini, sistem rekomendasi muncul sebagai alat yang powerful untuk memberikan saran buku yang dipersonalisasi [[1]](https://www.irjmets.com/uploadedfiles/paper//issue_8_august_2023/44243/final/fin_irjmets1692990447.pdf).

Saat ini, berbagai situs perpustakaan digital dan toko buku online bersaing satu sama lain dengan berbagai cara. Salah satu metode paling efektif untuk meningkatkan keuntungan dan mempertahankan pelanggan adalah sistem rekomendasi, yang dapat merekomendasikan buku yang menarik bagi pelanggan. Jadi, project ini membantu orang memilih buku yang tepat sesuai minat mereka dan mendorong mereka untuk membaca lebih banyak.  Dengan membangun sistem rekomendasi buku, dimaksudkan untuk mendukung orang-orang yang memiliki minat dalam membaca dan memengaruhi mereka yang menanamkan kebiasaan membaca [[2]](https://ieeexplore.ieee.org/abstract/document/9579647). 

## Business Understanding
### Problem Statement
1. Bagaimana memberikan rekomendasi buku berdasarkan penulis buku (author)?
2. Bagaimana memberikan rekomendasi buku berdasarkan data kolaboratif dari pengguna lain dengan preferensi yang serupa?

### Goals
1. Membuat sistem yang mampu merekomendasikan buku berdasarkan kemiripan judul dan penulis bukunya.
2. Membuat sistem yang mampu merekomendasikan buku berdasarkan data kolaboratif dari pengguna lain dengan preferensi yang serupa

## Data Understanding
Dataset yang digunakan pada project ini berasal dari Kaggle oleh [Möbius](https://www.kaggle.com/arashnic) yang dapat diunduh pada tautan berikut [Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset).

Dataset terdiri dari 3 file CSV berikut:
- Users.csv berisi informasi tentang pengguna. Pada dataset ini, ID pengguna (User-ID) telah diubah menjadi anonim dan dipetakan ke bilangan bulat. Data demografis seperti Location dan Age juga tersedia. Namun beberapa diantaranya berisi nilai NULL.
- Books.csv buku diidentifikasi oleh ISBN mereka masing-masing. ISBN yang tidak valid sudah dihapus dari dataset. Selain itu, beberapa informasi berbasis konten disediakan (Book-Title, Book-Author, Year-Of-Publication, Publisher), yang diperoleh dari Layanan Web Amazon. URL yang mengarah ke gambar sampul buku juga diberikan dalam tiga varian yang berbeda (Image-URL-S, Image-URL-M, Image-URL-L), yaitu kecil, sedang, dan besar. URL ini mengarah ke situs web Amazon.
- Ratings.csv berisi informasi peringkat buku. Peringkat (Book-Rating) dapat bersifat eksplisit, ditunjukkan dalam skala 1-10 (nilai yang lebih tinggi menunjukkan rating yang lebih tinggi), atau implisit, dinyatakan dengan 0.

Dataset books.csv memiliki jumlah total data unik sebagai berikut:
- Jumlah nomor ISBN buku: 271360
- Jumlah Book-Title: 242135
- Jumlah Book-Author: 102024
- Jumlah Year-Of-Publication: 202
- Jumlah Publisher: 16808

Berdasarkan dataframe books, jumlah judul buku dalam dataset adalah 242.135, sedangkan jumlah ISBN buku adalah 271.357. Hal ini menunjukkan adanya beberapa buku yang tidak memiliki nomor ISBN karena seharusnya setiap buku memiliki ISBN yang unik. Dalam hal ini, dataset akan difilter untuk memastikan setiap buku memiliki ISBN yang unik untuk modeling sistem rekomendasi Content Based.

**Grafik Penerbit dengan buku terbanyak**
![Penerbit dengan buku terbanyak](https://github.com/ainunannisak/ML-RecommendationSystem/assets/70701995/1835df6b-7366-4603-9aa2-89629c8ca848)

Gambar 1. Penerbit dengan buku terbanyak
Berdasarkan Gambar 1. di atas, diketahui bahwa penerbit Harlequin merilis buku paling banyak, dengan total lebih dari 7000 buku.

**Grafik Penulis dengan buku terbanyak**
![Penulis dengan buku terbanyak](https://github.com/ainunannisak/ML-RecommendationSystem/assets/70701995/dacb2898-3501-4793-8bd4-ec87cc07778c)

Gambar 2.  Penulis dengan buku terbanyak
Berdasarkan Gambar 2. di atas, diketahui bahwa penulis dengan nama Agatha Christie menulis buku paling banyak, dengan total lebih dari 600 buku. Dari informasi ini, juga terlihat bahwa dataset ini mencakup beberapa penulis yang telah menulis lebih dari satu judul buku.

## Data Preparation
Secara umum sebelum memasuki tahapan membuat model sistem rekomendasi, dilakukan persiapan data sebagai berikut:
- Menghapus kolom [Image-URL-S', 'Image-URL-M', 'Image-URL-L]  karena tidak diperlukan pada tahapan modeling.
  
  Tabel 1. Dataframe books
  
|   |       ISBN |                                        Book-Title |          Book-Author | Year-Of-Publication |                  Publisher |
|--:|-----------:|--------------------------------------------------:|---------------------:|--------------------:|---------------------------:|
| 0 | 0195153448 |                               Classical Mythology |   Mark P. O. Morford |                2002 |    Oxford University Press |
| 1 | 0002005018 |                                      Clara Callan | Richard Bruce Wright |                2001 |      HarperFlamingo Canada |
| 2 | 0060973129 |                              Decision in Normandy |         Carlo D'Este |                1991 |            HarperPerennial |
| 3 | 0374157065 | Flu: The Story of the Great Influenza Pandemic... |     Gina Bari Kolata |                1999 |       Farrar Straus Giroux |
| 4 | 0393045218 |                            The Mummies of Urumchi |      E. J. W. Barber |                1999 | W. W. Norton &amp; Company |

- Menggabungkan dataframe file books.csv dan ratings.csv berdasarkan ISBN.

  Tabel 2. Dataframe books setelah books.csv dan ratings.csv di merge
  
|   | User-ID |       ISBN | Book-Rating |                                        Book-Title |     Book-Author | Year-Of-Publication |                  Publisher |
|--:|--------:|-----------:|------------:|--------------------------------------------------:|----------------:|--------------------:|---------------------------:|
| 0 |  276725 | 034545104X |           0 |                              Flesh Tones: A Novel |      M. J. Rose |                2002 |           Ballantine Books |
| 1 |  276726 | 0155061224 |           5 |                                  Rites of Passage |      Judith Rae |                2001 |                     Heinle |
| 2 |  276727 | 0446520802 |           0 |                                      The Notebook | Nicholas Sparks |                1996 |               Warner Books |
| 3 |  276729 | 052165615X |           3 |                                    Help!: Level 1 |   Philip Prowse |                1999 | Cambridge University Press |
| 4 |  276729 | 0521795028 |           6 | The Amsterdam Connection : Level 4 (Cambridge ... |     Sue Leather |                2001 | Cambridge University Press |

- Memeriksa apakah ada missing values dengan mengecek setiap sel dalam dataset yang memiliki nilai kosong (null). Karena ada data dengan nilai koson maka data tersebut dihapus.
  
**Persiapan data untuk model sistem rekomendasi Content Based**
- Memeriksa duplikasi pada data ISBN. Ini dilakukan karena setelah dataframe digabungkan maka data ISBN menjadi duplikat banyak. Oleh karena itu, data duplikat berdasarkan ISBN dihapus.
- Mengambil hanya 10.000 sampel data untuk diproses lebih lanjut oleh model sistem rekomendasi
  
**Persiapan data untuk model sistem rekomendasi Collaborative Filtering**
- Mengumpulkan User-ID yang telah memberikan rating buku minimal sebanyak 200 buku
- Mengumpulkan Book-Title yang telah menerima rating minimal sebanyak 50 rating

Keduanya dilakukan agar kita memiliki pembaca berpengalaman serta daftar buku dengan jumlah peringkat yang cukup untuk setiap buku.

## Modeling and Result
### Content Based 
Pada tahap ini, akan dibuat sebuah model dengan menggunakan teknik Content-Based Filtering. Content-Based Filtering adalah pendekatan dalam sistem rekomendasi yang memanfaatkan informasi atau "konten" dari item atau pengguna untuk memberikan rekomendasi. Ide dasarnya adalah mencocokkan preferensi pengguna dengan karakteristik atau konten dari item yang telah dilihat atau disukai pengguna sebelumnya. Sebagai contoh, jika seorang pengguna menyukai atau telah membeli buku berjudul "Introduction to Machine Learning," dan buku tersebut memiliki fitur seperti nama penulis "Alex Smola," sistem akan mencari buku-buku lain dengan fitur serupa dan memberikan rekomendasi top-N kepada pengguna.
- Kelebihan: Personalisasi yang baik karena didasarkan pada preferensi unik dari setiap pengguna, tidak bergantung pada data pengguna lain, dan latar belakang yang lebih baik mengenai alasan di balik suatu rekomendasi karena didasarkan pada fitur atau konten spesifik dari item.
- Kelebihan: Jika data pengguna tidak lengkap atau tidak mencerminkan preferensi yang sebenarnya, sistem ini mungkin kesulitan memberikan rekomendasi yang akurat, ini tentunya juga mengakibatkan keterbatasan dalam menyesuaikan preferensi dari pengguna yang berubah-ubah.

Untuk menghitung tingkat kemiripan antara judul buku, digunakan teknik cosine similarity. Metode ini digunakan untuk mengukur kemiripan antara dua vektor dalam ruang berdimensi tinggi. Cosine similarity mengukur sudut kosinus antara dua vektor, dan semakin kecil sudutnya, semakin tinggi kemiripan antara vektor-vektor tersebut [[3]](https://journals.usm.ac.id/index.php/transformatika/article/view/1613).

Pada model Content Based Filtering ini akan memberikan rekomendasi berdasarkan penulis buku (Author)
Hasil rekomendasi 5 teratas pada judul buku yang dimasukkan sebagai berikut
```
book_title_to_recommend = "Mog's Christmas"
```
Tabel 3. Hasil rekomendasi Content Based berdasarkan penulis buku 

|   |                                           Recommended Book |       Author |
|--:|-----------------------------------------------------------:|-------------:|
| 0 | Paddington in the Kitchen                                  | Michael Bond |
| 1 | Paddington and the Marmalade Maze (Paddington First Books) | Michael Bond |
| 2 | Paddington at the Tower (A Paddington Picture Book)        | Michael Bond |
| 3 | The Adventures of Paddington                               | Michael Bond |
| 4 | PADDINGTON GOES TO SALES L/CUB (Collins Colour Cubs)       | Michael Bond |

### Collaborative Filtering
Collaborative Filtering adalah salah satu metode dalam sistem rekomendasi yang mengandalkan informasi dari pengguna lain untuk memberikan rekomendasi pada suatu pengguna. Metode ini memanfaatkan preferensi atau perilaku pengguna untuk menentukan rekomendasi bagi pengguna lain yang memiliki kesamaan atau kesamaan selera. Terdapat dua jenis utama dari Collaborative Filtering, yaitu User-Based Collaborative Filtering dan Item-Based Collaborative Filtering.

Dalam proses training model menghitung skor kesesuaian antara pengguna dan judul buku menggunakan teknik embedding. Selanjutnya, dilakukan operasi perkalian dot product antara embedding pengguna dan judul buku. Selain itu, ditambahkan bias untuk setiap pengguna dan judul buku. Skor kesesuaian diatur dalam skala [0,1] dengan fungsi aktivasi sigmoid. Model dibuat dengan kelas RecommenderNet menggunakan kelas Model dari Keras. Model akan menggunakan Binary Crossentropy untuk menghitung fungsi loss, Adam (Adaptive Moment Estimation) sebagai Optimalizer, dan Root Mean Squared Error (RMSE) sebagai metrik evaluasi.

Berikut hasil rekomendasi 10 buku teratas dengan model Collaborative Filtering

```
Rekomendasi untuk User ID: 209516
```

Tabel 4. Hasil rekomendasi Collaborative Filtering
|   |                                        Book Title |             Book Author |
|--:|--------------------------------------------------:|------------------------:|
| 0 | Stupid White Men ...and Other Sorry Excuses fo... |           Michael Moore |
| 1 |                                   Brave New World |           Aldous Huxley |
| 2 |                                           Riptide |         Mickey Friedman |
| 3 |                          I Know This Much Is True |              Wally Lamb |
| 4 | The Alchemist: A Fable About Following Your Dream |            Paulo Coelho |
| 5 |                  Charlotte's Web (Trophy Newbery) |             E. B. White |
| 6 |                                 The Secret Garden | Frances Hodgson Burnett |
| 7 |                                          Vanished |     Mary McGarry Morris |
| 8 |           The Girls' Guide to Hunting and Fishing |            Melissa Bank |
| 9 |                                       About a Boy |             Nick Hornby |

## Evaluation
### Content Based Evaluation
Buku yang direkomendasikan pada model ini berdasarkan kesamaan/kemiripan dari penulis buku (Author), dengan ini dilakukan perhitungan dengan Precision. Presisi (Precision) dapat dihitung dengan rumus berikut:

![image](https://github.com/ainunannisak/ML-RecommendationSystem/assets/70701995/8acf73ab-91cf-453f-ba5c-9805e9d8c262)

Gambar 3. Perhitungan Precision Model Content Based

Karena seluruh item (5 item) yang direkomendasikan relevan, maka nilai presisinya adalah 100%.

### Collaborative Filtering Evaluation
Model Collaborative Filtering dievaluasi menggunakan metrik Root Mean Square Error (RMSE). RMSE digunakan untuk mengevaluasi sejauh mana perbedaan antara rating sebenarnya yang diberikan oleh pengguna dan rating yang diprediksi oleh sistem rekomendasi.

![image](https://github.com/ainunannisak/ML-RecommendationSystem/assets/70701995/6825ab72-391b-49a1-9596-5071dc9440a1)

Gambar 4. Metrik RMSE Training dan Validation

Berdasarka Gambar 4. diatas dapat dilihat bahwa model mencapai 100 epochs. Melihat plot metrik model menunjukkan nilai MSE yang relatif kecil. Proses ini menghasilkan nilai kesalahan akhir sebesar 0.3232, dengan data validasi menunjukkan kesalahan sebesar 0.3388. Angka-angka ini menunjukkan hasil yang baik untuk sistem rekomendasi yang dibangun. Semakin rendah nilai RMSE, semakin baik kemampuan model dalam memprediksi preferensi pengguna, sehingga meningkatkan akurasi hasil rekomendasi.


## References

[1] C. DM, K. K. Sumalatha, G. V. Sandya, dan S. Goravanakolla, "Content Based Book Recommendation System," International Research Journal of Modernization in Engineering Technology and Science, vol. 05, no. 08, pp. 1988, August 2023, ISSN: 2582-5208, Impact Factor: 7.868, [Online]. Available: www.irjmets.com.

[2] P. Devika, K. Jyothisree, P. Rahul, S. Arjun and J. Narayanan, "Book Recommendation System," 2021 12th International Conference on Computing Communication and Networking Technologies (ICCCNT), Kharagpur, India, 2021, pp. 1-5, doi: 10.1109/ICCCNT51525.2021.9579647. 

[3] D. Kurniadi, S. F. C. Haviana, dan A. Novianto, "Implementasi Algoritma Cosine Similarity pada sistem arsip dokumen di Universitas Islam Sultan Agung," Jurnal Transformatika, vol. 17, no. 2, pp. 124-133, Jan. 2020. [Online]. Available: https://journals.usm.ac.id/index.php/transformatika/article/view/1613. Diakses pada: 05 Mar. 2024. doi: 10.26623/transformatika.v17i2.1613.


