# ML-RecommendationSystem

## Project Overview
Perpustakaan digital dan toko buku online semakin banyak dan mudah untuk diakses. Ini memberikan pembaca keleluasaan untuk memilih, namun juga memberikan tantangan untuk menemukan buku yang sesuai dengan minat dan preferensi mereka. Untuk mengatasi situasi ini, sistem rekomendasi muncul sebagai alat yang powerful untuk memberikan saran buku yang dipersonalisasi [[1]](https://www.irjmets.com/uploadedfiles/paper//issue_8_august_2023/44243/final/fin_irjmets1692990447.pdf).

Saat ini, berbagai situs perpustakaan digital dan toko buku online bersaing satu sama lain dengan berbagai cara. Salah satu metode paling efektif untuk meningkatkan keuntungan dan mempertahankan pelanggan adalah sistem rekomendasi, yang dapat merekomendasikan buku yang menarik bagi pelanggan. Jadi, project ini membantu orang memilih buku yang tepat sesuai minat mereka dan mendorong mereka untuk membaca lebih banyak.  Dengan membangun sistem rekomendasi buku, dimaksudkan untuk mendukung orang-orang yang memiliki minat dalam membaca dan memengaruhi mereka yang menanamkan kebiasaan membaca [[2]](https://ieeexplore.ieee.org/abstract/document/9579647). 

## Business Understanding
### Problem Statement
1. Bagaimana memberikan rekomendasi buku berdasarkan kemiripan judul dan penulis buku?
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

## Data Preparation

## Modeling and Result

## Evaluation

## References

[1] C. DM, K. K. Sumalatha, G. V. Sandya, dan S. Goravanakolla, "Content Based Book Recommendation System," International Research Journal of Modernization in Engineering Technology and Science, vol. 05, no. 08, pp. 1988, August 2023, ISSN: 2582-5208, Impact Factor: 7.868, [Online]. Available: www.irjmets.com.

[2] P. Devika, K. Jyothisree, P. Rahul, S. Arjun and J. Narayanan, "Book Recommendation System," 2021 12th International Conference on Computing Communication and Networking Technologies (ICCCNT), Kharagpur, India, 2021, pp. 1-5, doi: 10.1109/ICCCNT51525.2021.9579647. 



