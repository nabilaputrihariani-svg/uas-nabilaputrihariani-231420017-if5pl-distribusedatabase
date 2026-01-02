# UAS Distributed Database MySQL
## Sistem Database Pegawai Perusahaan Multi Cabang

Nama  : Nabila Putri Hariani  
NIM   : 231420017  
Mata Kuliah : Distributed Database  
DBMS  : MySQL  

---

## 1. Deskripsi Sistem
Sebuah perusahaan memiliki banyak cabang yang tersebar di beberapa kota,
yaitu Jakarta, Bandung, dan Surabaya.
Setiap cabang memiliki data pegawai yang dikelola secara lokal sesuai
dengan lokasi masing-masing.

Untuk meningkatkan kinerja sistem, database pegawai didesain menggunakan
konsep Distributed Database dengan tujuan:
- Penggunaan data yang lebih efisien
- Akses data lebih cepat di masing-masing cabang
- Meningkatkan keamanan data
- Memungkinkan proses paralel

Meskipun data tersebar di beberapa server, sistem tetap dapat diakses
seolah-olah sebagai satu database terpusat.

---

## 2. Arsitektur Distributed Database
Sistem database dibagi menjadi tiga server berdasarkan lokasi cabang:
- Server Jakarta
- Server Bandung
- Server Surabaya

Setiap server menyimpan data pegawai sesuai dengan kota cabang masing-masing.

---

## 3. Metode Distribusi Data
Metode distribusi data yang digunakan adalah **fragmentasi horizontal**,
yaitu pembagian data berdasarkan baris (record) dengan kriteria atribut `kota`.

Contoh:
- Data pegawai dengan kota Jakarta disimpan di Server Jakarta
- Data pegawai dengan kota Bandung disimpan di Server Bandung
- Data pegawai dengan kota Surabaya disimpan di Server Surabaya

---

## 4. Desain Tabel
Struktur tabel pada setiap server adalah sama.

Nama Tabel: `pegawai`

| Nama Kolom  | Tipe Data      | Keterangan        |
|------------|---------------|------------------|
| id_pegawai | INT           | Primary Key      |
| nama       | VARCHAR(50)   | Nama pegawai     |
| jabatan    | VARCHAR(50)   | Jabatan pegawai  |
| gaji       | INT           | Gaji pegawai     |
| kota       | VARCHAR(30)   | Kota cabang      |

---

## 5. Pembagian Data per Server
### Server Jakarta
- Budi – Admin
- Andi – Staff

### Server Bandung
- Siti – Manager
- Dimas – Admin

### Server Surabaya
- Rina – Supervisor

---

## 6. Implementasi Database
Setiap server memiliki database MySQL sendiri dengan struktur tabel yang sama.
File SQL disediakan untuk masing-masing server:
- `server-jakarta.sql`
- `server-bandung.sql`
- `server-surabaya.sql`

---

## 7. Contoh Query Gabungan
Untuk menampilkan seluruh data pegawai dari semua server,
digunakan query UNION sebagai berikut:

```sql
SELECT * FROM db_jakarta.pegawai
UNION
SELECT * FROM db_bandung.pegawai
UNION
SELECT * FROM db_surabaya.pegawai;
