# Distributed Database Sample
## Sistem Database Pegawai Perusahaan Multi Cabang (MySQL)

Nama  : Nabila Putri Hariani  
NIM   : 231420017  
Mata Kuliah : Distributed Database  
DBMS  : MySQL  

---

## Overview

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

## Business Scenario

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

## Objectives

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

## Distributed Database Architecture

### Logical Architecture
Secara logis, sistem database pegawai ini dipandang sebagai **satu database
terintegrasi** oleh pengguna. Pengguna tidak perlu mengetahui lokasi fisik
penyimpanan data, karena sistem menyediakan transparansi distribusi data.

### Physical Architecture
Secara fisik, database didistribusikan ke beberapa server berdasarkan lokasi
cabang perusahaan. Setiap server memiliki database MySQL sendiri.

## Schema Diagram
Schema diagram ini menunjukkan struktur dan relasi tabel pada sistem distributed
database pegawai perusahaan multi cabang yang dibagi berdasarkan lokasi cabang
(Jakarta, Bandung, dan Surabaya).
![Schema Diagram Distributed Database](schema-diagram.png.jpeg)

Setiap server menyimpan data pegawai sesuai dengan lokasi cabangnya masing-masing.

---

## Database Schema Description

Database terdiri dari beberapa tabel utama yang saling berelasi, yaitu:
- `cabang`
- `jabatan`
- `pegawai`
- `penggajian`

---

### Table: cabang

Tabel `cabang` menyimpan data lokasi cabang perusahaan.

| Column Name | Data Type   | Key | Null | Description |
|------------|------------|-----|------|-------------|
| id_cabang  | INT        | PK  | NO   | ID unik cabang |
| nama_cabang| VARCHAR(50)| -   | NO   | Nama cabang |
| kota       | VARCHAR(30)| -   | NO   | Kota lokasi cabang |

Tabel ini direplikasi ke seluruh server untuk menjaga konsistensi data referensi.

---

### Table: jabatan

Tabel `jabatan` menyimpan daftar jabatan pegawai.

| Column Name | Data Type   | Key | Null | Description |
|------------|------------|-----|------|-------------|
| id_jabatan | INT        | PK  | NO   | ID unik jabatan |
| nama_jabatan | VARCHAR(50) | - | NO | Nama jabatan |
| gaji_pokok | INT        | -   | NO   | Gaji pokok jabatan |

Tabel `jabatan` juga bersifat replicated table.

---

### Table: pegawai

Tabel `pegawai` merupakan tabel utama yang menyimpan data pegawai.

| Column Name | Data Type   | Key | Null | Description |
|------------|------------|-----|------|-------------|
| id_pegawai | INT        | PK  | NO   | ID unik pegawai |
| nama       | VARCHAR(50)| -   | NO   | Nama lengkap pegawai |
| id_jabatan | INT        | FK  | NO   | Relasi ke tabel jabatan |
| id_cabang  | INT        | FK  | NO   | Relasi ke tabel cabang |
| kota       | VARCHAR(30)| -   | NO   | Lokasi cabang pegawai |

Tabel `pegawai` didistribusikan secara **horizontal** berdasarkan atribut `kota`.

---

### Table: penggajian

Tabel `penggajian` menyimpan data gaji pegawai per periode.

| Column Name | Data Type   | Key | Null | Description |
|------------|------------|-----|------|-------------|
| id_gaji    | INT        | PK  | NO   | ID unik penggajian |
| id_pegawai | INT        | FK  | NO   | Relasi ke tabel pegawai |
| bulan      | VARCHAR(15)| -   | NO   | Bulan penggajian |
| tahun      | INT        | -   | NO   | Tahun penggajian |
| total_gaji | INT        | -   | NO   | Total gaji pegawai |

Data penggajian disimpan pada server cabang masing-masing.

---

## Table Relationships

Relasi antar tabel adalah sebagai berikut:
- `pegawai.id_jabatan` → `jabatan.id_jabatan`
- `pegawai.id_cabang` → `cabang.id_cabang`
- `penggajian.id_pegawai` → `pegawai.id_pegawai`

Relasi ini menjaga integritas data dan konsistensi antar tabel.

---
## Table Relationships

Hubungan antar tabel pada sistem distributed database pegawai dirancang untuk
menjaga integritas data dan mendukung pengelolaan data terdistribusi. Relasi antar
tabel ditunjukkan pada tabel berikut:

### Relationship Summary

| Parent Table | Child Table  | Relationship Type | Foreign Key       | Description |
|-------------|--------------|-------------------|-------------------|-------------|
| cabang      | pegawai      | One-to-Many       | pegawai.id_cabang | Setiap cabang memiliki banyak pegawai |
| jabatan     | pegawai      | One-to-Many       | pegawai.id_jabatan| Setiap jabatan dapat dimiliki banyak pegawai |
| pegawai     | penggajian   | One-to-Many       | penggajian.id_pegawai | Setiap pegawai memiliki data penggajian |

---

### cabang – pegawai

Relasi antara tabel `cabang` dan `pegawai` menunjukkan bahwa setiap pegawai
terdaftar pada satu cabang tertentu.

- **Primary Key**: `cabang.id_cabang`
- **Foreign Key**: `pegawai.id_cabang`
- **Relationship Type**: One-to-Many

Relasi ini digunakan sebagai dasar fragmentasi data pegawai berdasarkan lokasi
cabang.

---

### jabatan – pegawai

Relasi antara tabel `jabatan` dan `pegawai` digunakan untuk menentukan posisi
atau jabatan pegawai dalam perusahaan.

- **Primary Key**: `jabatan.id_jabatan`
- **Foreign Key**: `pegawai.id_jabatan`
- **Relationship Type**: One-to-Many

Relasi ini memastikan konsistensi data jabatan pada seluruh server database.

---

### pegawai – penggajian

Relasi antara tabel `pegawai` dan `penggajian` digunakan untuk menyimpan riwayat
penggajian pegawai.

- **Primary Key**: `pegawai.id_pegawai`
- **Foreign Key**: `penggajian.id_pegawai`
- **Relationship Type**: One-to-Many

Data penggajian disimpan mengikuti lokasi cabang pegawai.
## Distributed Database Strategy

Strategi distributed database yang diterapkan meliputi:
- **Horizontal Fragmentation** pada tabel `pegawai` dan `penggajian`
- **Replication** pada tabel `cabang` dan `jabatan`
- **Local Access** untuk kebutuhan operasional cabang
- **Global Access** untuk laporan manajemen pusat

Pendekatan ini membuat sistem database lebih efisien, aman, dan scalable.

---

## Conclusion

Implementasi distributed database pada sistem pegawai perusahaan multi cabang
memungkinkan pengelolaan data yang lebih efisien, cepat, dan aman. Dengan
pendistribusian data berdasarkan lokasi cabang, sistem dapat mendukung kebutuhan
operasional dan manajerial secara optimal.




