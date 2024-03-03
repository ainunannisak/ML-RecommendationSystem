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

Grafik Penerbit dengan buku terbanyak
![Penerbit dengan buku terbanyak](https://github.com/ainunannisak/ML-RecommendationSystem/assets/70701995/1835df6b-7366-4603-9aa2-89629c8ca848)

Gambar 1.  Penerbit dengan buku terbanyak
Berdasarkan Gambar 1. di atas, diketahui bahwa penerbit Harlequin merilis buku paling banyak, dengan total lebih dari 7000 buku.

Grafik Penulis dengan buku terbanyak
![Penulis dengan buku terbanyak](https://github.com/ainunannisak/ML-RecommendationSystem/assets/70701995/dacb2898-3501-4793-8bd4-ec87cc07778c)

Gambar 2.  Penulis dengan buku terbanyak
Berdasarkan Gambar 2. di atas, diketahui bahwa penulis dengan nama Agatha Christie menulis buku paling banyak, dengan total lebih dari 600 buku. Dari informasi ini, juga terlihat bahwa dataset ini mencakup beberapa penulis yang telah menulis lebih dari satu judul buku.

## Data Preparation
Secara umum sebelum memasuki tahapan membuat model sistem rekomendasi, dilakukan persiapan data sebagai berikut:
- Menghapus kolom [Image-URL-S', 'Image-URL-M', 'Image-URL-L]  karena tidak diperlukan pada tahapan modeling.
- Menggabungkan dataframe file books.csv dan ratings.csv berdasarkan ISBN.
- Memeriksa apakah ada missing values dengan mengecek setiap sel dalam dataset yang memiliki nilai kosong (null). Karena ada data dengan nilai koson maka data tersebut dihapus.
  
**Persiapan data untuk model sistem rekomendasi Content Based**
- Memeriksa duplikasi pada data ISBN. Ini dilakukan karena setelah dataframe digabungkan maka data ISBN menjadi duplikat banyak. Oleh karena itu, data duplikat berdasarkan ISBN dihapus.
- Mengambil hanya 10.000 sampel data untuk diproses lebih lanjut oleh model sistem rekomendasi
  
**Persiapan data untuk model sistem rekomendasi Collaborative Filtering**
- Mengumpulkan User-ID yang telah memberikan rating buku minimal sebanyak 200 buku
- Mengumpulkan Book-Title yang telah menerima rating minimal sebanyak 50 rating
Keduanya dilakukan agar kita memiliki pembaca berpengalaman serta daftar buku dengan jumlah peringkat yang cukup untuk setiap buku.

## Modeling and Result

## Evaluation

## References

[1] C. DM, K. K. Sumalatha, G. V. Sandya, dan S. Goravanakolla, "Content Based Book Recommendation System," International Research Journal of Modernization in Engineering Technology and Science, vol. 05, no. 08, pp. 1988, August 2023, ISSN: 2582-5208, Impact Factor: 7.868, [Online]. Available: www.irjmets.com.

[2] P. Devika, K. Jyothisree, P. Rahul, S. Arjun and J. Narayanan, "Book Recommendation System," 2021 12th International Conference on Computing Communication and Networking Technologies (ICCCNT), Kharagpur, India, 2021, pp. 1-5, doi: 10.1109/ICCCNT51525.2021.9579647. 



