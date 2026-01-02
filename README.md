# Distributed Database Sample
## Sistem Database Pegawai Perusahaan Multi Cabang (MySQL)

Nama  : Nabila Putri Hariani  
NIM   : 231420017  
Mata Kuliah : Distributed Database  
DBMS  : MySQL  

---

## 1. Overview

Repository ini berisi contoh implementasi **Distributed Database** menggunakan
**MySQL** dengan studi kasus **database pegawai pada perusahaan multi cabang**.
Sistem ini dirancang untuk menunjukkan bagaimana data dapat disimpan dan
dikelola secara terdistribusi di beberapa lokasi fisik yang berbeda, namun tetap
dapat diakses sebagai satu kesatuan sistem oleh pengguna.

Pada implementasi ini, data pegawai tidak disimpan pada satu server pusat,
melainkan didistribusikan ke beberapa server berdasarkan lokasi cabang
perusahaan. Pendekatan ini bertujuan untuk meningkatkan performa sistem,
efisiensi pengelolaan data, serta keandalan database secara keseluruhan.

Distributed database yang diterapkan bersifat **homogeneous**, karena seluruh
node database menggunakan **DBMS yang sama, yaitu MySQL**. Meskipun proyek ini
bersifat simulasi akademik, desain yang digunakan merepresentasikan konsep
distributed database yang umum diterapkan pada sistem skala besar di dunia
nyata, seperti perusahaan nasional, sistem perbankan, dan organisasi dengan
banyak cabang.

Repository ini dilengkapi dengan dokumentasi arsitektur sistem, strategi
distribusi data, desain skema database, serta contoh query SQL untuk menunjukkan
bagaimana data dari beberapa server dapat diakses secara terintegrasi.

---

## 2. Business Scenario

Sebuah perusahaan memiliki beberapa cabang yang tersebar di kota-kota besar,
yaitu **Jakarta, Bandung, dan Surabaya**. Setiap cabang memiliki aktivitas
operasional sendiri dan bertanggung jawab dalam pengelolaan data pegawai yang
bekerja di cabang tersebut.

Pada sistem database terpusat, seluruh data pegawai disimpan pada satu server
utama. Pendekatan ini menimbulkan berbagai permasalahan, seperti:
- Beban akses yang tinggi pada satu server
- Waktu respon yang lebih lambat untuk cabang yang jauh dari server pusat
- Risiko kegagalan sistem yang tinggi apabila server pusat mengalami gangguan

Untuk mengatasi permasalahan tersebut, perusahaan menerapkan konsep
**Distributed Database**, di mana setiap cabang memiliki server database sendiri
yang menyimpan data pegawai sesuai dengan lokasi cabang tersebut. Meskipun data
disimpan secara terpisah, manajemen pusat tetap dapat mengakses seluruh data
pegawai secara terintegrasi untuk keperluan monitoring, pelaporan, dan
pengambilan keputusan.

---

## 3. Objectives

Penerapan Distributed Database pada sistem database pegawai perusahaan ini
memiliki beberapa tujuan utama sebagai berikut:

- Meningkatkan efisiensi penggunaan data  
- Mempercepat akses data di masing-masing cabang  
- Meningkatkan keamanan data  
- Mendukung pemrosesan data secara paralel  
- Meningkatkan keandalan dan skalabilitas sistem  

Dengan tercapainya tujuan-tujuan tersebut, sistem distributed database ini
diharapkan mampu memberikan solusi yang lebih optimal dibandingkan dengan
sistem database terpusat.

---

## 4. Distributed Database Architecture

### 4.1 Logical Architecture
Secara logis, sistem database pegawai ini dipandang sebagai **satu database
terintegrasi** oleh pengguna. Pengguna tidak perlu mengetahui lokasi fisik
penyimpanan data, karena sistem menyediakan transparansi distribusi data.

### 4.2 Physical Architecture
Secara fisik, database didistribusikan ke beberapa server berdasarkan lokasi
cabang perusahaan. Setiap server memiliki database MySQL sendiri.
![Distributed Database Architecture](architecture.png)

            
+---------------+ +---------------+ +---------------+
