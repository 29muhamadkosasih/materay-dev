# Materi Dasar Database

Dokumen ini berisi materi dasar tentang database yang disusun lebih rapi, lengkap, dan mudah dipahami, terutama untuk pemula yang sedang belajar pengembangan aplikasi seperti PHP, Laravel, atau sistem informasi pada umumnya.

---

## 1. Pengertian Database

**Database** adalah kumpulan data yang disimpan secara terstruktur di dalam komputer sehingga data tersebut dapat dikelola, diakses, dicari, diperbarui, dan dihapus dengan mudah.

### Pengertian sederhananya

Database bisa dianggap sebagai tempat penyimpanan data digital yang terorganisir. Jika pada dunia nyata kita menyimpan arsip di lemari atau map, maka dalam dunia aplikasi kita menyimpan data di dalam database.

### Contoh data yang biasanya disimpan di database

- data pengguna,
- data produk,
- data transaksi,
- data absensi,
- data nilai,
- data laporan,
- dan lain-lain.

### Contoh penggunaan database

- sistem absensi,
- sistem penjualan,
- sistem akademik,
- sistem perpustakaan,
- aplikasi perbankan,
- e-commerce,
- aplikasi rumah sakit.

### Contoh sederhana database

Misalnya ada tabel bernama `mahasiswa`:

| ID  | Nama | Umur |
| --- | ---- | ---- |
| 1   | Andi | 20   |
| 2   | Budi | 22   |
| 3   | Siti | 19   |

Data di atas disimpan dalam satu tabel dan bisa diolah kapan saja sesuai kebutuhan aplikasi.

---

## 2. Kenapa Database Penting?

Database sangat penting karena hampir semua aplikasi modern membutuhkan penyimpanan data yang teratur.

### Fungsi utama database

- menyimpan data secara permanen,
- memudahkan pencarian data,
- memudahkan pembaruan data,
- menjaga data tetap rapi,
- membantu hubungan antar data,
- mendukung laporan dan analisis.

### Contoh tanpa database

Jika sebuah aplikasi tidak memakai database, maka data hanya tersimpan sementara atau harus ditulis manual di file biasa. Ini akan membuat data:

- sulit dicari,
- mudah rusak,
- sulit dihubungkan,
- tidak efisien,
- sulit dipakai banyak user.

---

## 3. Komponen Utama Database

Dalam database, ada beberapa komponen dasar yang wajib dipahami.

### 1. Table (Tabel)

Tabel adalah tempat menyimpan data dalam bentuk **baris dan kolom**.

Contoh tabel `users`:

| id  | nama | email         |
| --- | ---- | ------------- |
| 1   | Andi | andi@mail.com |
| 2   | Budi | budi@mail.com |

### Pengertian tambahan

Satu tabel biasanya mewakili satu jenis data. Contohnya:

- tabel `users` untuk data pengguna,
- tabel `products` untuk data produk,
- tabel `orders` untuk data pesanan.

---

### 2. Field / Column (Kolom)

Field atau column adalah atribut dari sebuah tabel.

Contoh field pada tabel `users`:

| Field |
| ----- |
| id    |
| nama  |
| email |

### Pengertian tambahan

Kolom menjelaskan jenis informasi yang disimpan pada tabel.

Contoh:

- `nama` menyimpan nama user,
- `email` menyimpan email user,
- `created_at` menyimpan waktu data dibuat.

---

### 3. Record / Row (Baris)

Record adalah satu baris data lengkap dalam tabel.

Contoh satu record pada tabel `users`:

| id  | nama | email         |
| --- | ---- | ------------- |
| 1   | Andi | andi@mail.com |

### Pengertian tambahan

Jika kolom adalah jenis datanya, maka record adalah isi datanya.

---

### 4. Primary Key

Primary key adalah kolom yang berfungsi sebagai identitas unik untuk setiap record.

Contoh primary key:

```text
id
```

Contoh tabel:

| id  | nama |
| --- | ---- |
| 1   | Andi |
| 2   | Budi |

Pada tabel di atas, `id` adalah primary key.

### Kenapa primary key penting?

- agar setiap data memiliki identitas unik,
- mencegah data ganda pada identitas utama,
- memudahkan pencarian data,
- mempermudah relasi antar tabel.

---

### 5. Foreign Key

Foreign key adalah kolom yang digunakan untuk menghubungkan satu tabel dengan tabel lain.

Contoh:

#### Tabel `users`

| id  | nama |
| --- | ---- |
| 1   | Andi |
| 2   | Budi |

#### Tabel `orders`

| id  | user_id | produk |
| --- | ------- | ------ |
| 1   | 1       | Laptop |
| 2   | 2       | Mouse  |

`user_id` pada tabel `orders` adalah foreign key yang mengarah ke `users.id`.

### Pengertian tambahan

Foreign key membantu database memahami bahwa sebuah pesanan dimiliki oleh user tertentu.

---

## 4. Database Management System (DBMS)

**DBMS** adalah software yang digunakan untuk membuat, mengelola, dan mengakses database.

Tanpa DBMS, database tidak dapat dikelola dengan baik.

### Contoh DBMS populer

- MySQL
- PostgreSQL
- Microsoft SQL Server
- Oracle Database
- SQLite
- MongoDB

### Yang sering dipakai pada PHP dan Laravel

Yang paling sering digunakan adalah **MySQL** dan **PostgreSQL**.

### Pengertian tambahan

DBMS bertugas membantu proses seperti:

- membuat database,
- membuat tabel,
- menambah data,
- mengubah data,
- menghapus data,
- menjaga keamanan data,
- mengatur relasi data.

---

## 5. Jenis-Jenis Database

Secara umum, database dapat dibagi menjadi dua kategori besar.

### 1. Relational Database

Relational database adalah database berbasis tabel yang saling berhubungan.

### Contoh

- MySQL
- PostgreSQL
- SQL Server
- Oracle

### Ciri-ciri

- menggunakan tabel,
- menggunakan SQL,
- memiliki relasi antar tabel,
- struktur data lebih jelas dan teratur.

### Cocok digunakan untuk

- aplikasi sekolah,
- sistem penjualan,
- aplikasi keuangan,
- aplikasi inventaris,
- sistem akademik.

---

### 2. NoSQL Database

NoSQL adalah jenis database yang tidak selalu menggunakan struktur tabel seperti relational database.

### Contoh

- MongoDB
- Firebase
- Cassandra

### Ciri-ciri

- lebih fleksibel untuk data tertentu,
- cocok untuk data yang tidak terstruktur atau semi-terstruktur,
- sering dipakai pada aplikasi real-time atau skala besar.

### Cocok digunakan untuk

- big data,
- chat real-time,
- aplikasi IoT,
- data dokumen,
- aplikasi dengan struktur data yang sangat dinamis.

---

## 6. Struktur Database Sederhana

Agar lebih mudah dipahami, berikut contoh struktur database untuk sistem penjualan sederhana.

### Tabel `users`

| id  | nama | email         |
| --- | ---- | ------------- |
| 1   | Andi | andi@mail.com |

### Tabel `produk`

| id  | nama_produk | harga    |
| --- | ----------- | -------- |
| 1   | Laptop      | 10000000 |
| 2   | Mouse       | 100000   |

### Tabel `orders`

| id  | user_id | tanggal    |
| --- | ------- | ---------- |
| 1   | 1       | 2026-03-01 |

### Tabel `order_detail`

| id  | order_id | produk_id | qty |
| --- | -------- | --------- | --- |
| 1   | 1        | 2         | 3   |

### Relasi sederhananya

```text
users
   |
orders
   |
order_detail
   |
produk
```

### Pengertian tambahan

Dari struktur di atas:

- satu user bisa memiliki banyak order,
- satu order bisa memiliki banyak detail barang,
- satu produk bisa muncul di banyak transaksi.

---

## 7. SQL (Structured Query Language)

**SQL** adalah bahasa yang digunakan untuk berinteraksi dengan database.

Dengan SQL, kita bisa:

- membuat database,
- membuat tabel,
- menambah data,
- mengambil data,
- mengubah data,
- menghapus data,
- mengatur hak akses,
- mengatur struktur tabel.

### Pengertian tambahan

SQL adalah dasar yang sangat penting dalam belajar database. Meskipun nanti menggunakan framework seperti Laravel, konsep SQL tetap perlu dipahami karena semua operasi data pada akhirnya berhubungan dengan query database.

---

## 8. DDL (Data Definition Language)

DDL adalah bagian dari SQL yang digunakan untuk membuat atau mengubah struktur database.

### Contoh perintah DDL

- `CREATE` → membuat
- `ALTER` → mengubah struktur
- `DROP` → menghapus

### Membuat database

```sql
CREATE DATABASE toko;
```

### Membuat tabel

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    email VARCHAR(100)
);
```

### Mengubah tabel

```sql
ALTER TABLE users
ADD alamat TEXT;
```

### Menghapus tabel

```sql
DROP TABLE users;
```

### Pengertian tambahan

DDL fokus pada struktur, bukan isi data.

---

## 9. DML (Data Manipulation Language)

DML adalah bagian dari SQL yang digunakan untuk mengelola isi data di dalam tabel.

### Contoh perintah DML

- `INSERT` → menambah data
- `SELECT` → mengambil data
- `UPDATE` → mengubah data
- `DELETE` → menghapus data

### INSERT (Menambah Data)

```sql
INSERT INTO users (nama, email)
VALUES ('Andi', 'andi@mail.com');
```

### SELECT (Mengambil Data)

```sql
SELECT * FROM users;
```

Contoh hasil:

| id  | nama | email         |
| --- | ---- | ------------- |
| 1   | Andi | andi@mail.com |

### UPDATE (Mengubah Data)

```sql
UPDATE users
SET nama = 'Andi Saputra'
WHERE id = 1;
```

### DELETE (Menghapus Data)

```sql
DELETE FROM users
WHERE id = 1;
```

### Pengertian tambahan

DML adalah perintah yang paling sering dipakai dalam aplikasi karena hampir semua sistem selalu menambah, membaca, mengubah, dan menghapus data.

---

## 10. Relasi Database

Relasi database digunakan untuk menghubungkan tabel satu dengan tabel lainnya.

Relasi membuat data menjadi lebih rapi, tidak berulang, dan lebih mudah dikelola.

### 1. One To One

Satu data pada tabel A berhubungan dengan satu data pada tabel B.

### Contoh

```text
users
profiles
```

Contoh kasus:

- satu user memiliki satu profil,
- satu profil hanya milik satu user.

---

### 2. One To Many

Satu data pada tabel A berhubungan dengan banyak data pada tabel B.

### Contoh

```text
users -> orders
```

Contoh kasus:

- satu user bisa membuat banyak order,
- satu order hanya dimiliki satu user.

---

### 3. Many To Many

Banyak data pada tabel A berhubungan dengan banyak data pada tabel B.

### Contoh

```text
students -> courses
```

Biasanya membutuhkan tabel penghubung:

```text
student_course
```

Contoh kasus:

- satu siswa bisa mengambil banyak mata pelajaran,
- satu mata pelajaran bisa diikuti banyak siswa.

---

## 11. Normalisasi Database

Normalisasi adalah proses mengatur struktur tabel agar data tidak berulang secara berlebihan dan tetap konsisten.

### Tujuan normalisasi

- mengurangi duplikasi data,
- menjaga konsistensi data,
- mempermudah maintenance,
- membuat database lebih efisien.

### Contoh tidak normal

| order_id | nama_user | produk |
| -------- | --------- | ------ |
| 1        | Andi      | Laptop |

### Masalahnya

- nama user diulang,
- data sulit dirawat jika berubah,
- berpotensi menimbulkan inkonsistensi.

### Setelah dinormalisasi

#### Tabel `users`

| id  | nama |
| --- | ---- |
| 1   | Andi |

#### Tabel `orders`

| id  | user_id |
| --- | ------- |
| 1   | 1       |

### Pengertian tambahan

Dengan normalisasi, perubahan data user cukup dilakukan di satu tempat saja, yaitu pada tabel `users`.

---

## 12. Index Database

Index adalah fitur database yang digunakan untuk mempercepat proses pencarian data.

### Contoh membuat index

```sql
CREATE INDEX idx_email
ON users(email);
```

### Pengaruh index

Tanpa index:

- query pencarian bisa lebih lambat,
- database harus mencari data lebih banyak.

Dengan index:

- pencarian data lebih cepat,
- performa query tertentu meningkat.

### Catatan penting

Walaupun index membantu pencarian, terlalu banyak index juga bisa membuat proses `INSERT`, `UPDATE`, dan `DELETE` menjadi lebih berat.

---

## 13. Contoh Kasus Sistem Absensi

Misalnya kita membuat database untuk sistem absensi.

### Tabel `users`

| id  | nama |
| --- | ---- |
| 1   | Andi |

### Tabel `absensi`

| id  | user_id | tanggal    | status |
| --- | ------- | ---------- | ------ |
| 1   | 1       | 2026-03-01 | hadir  |

### Query untuk menampilkan data absensi

```sql
SELECT users.nama, absensi.tanggal, absensi.status
FROM absensi
JOIN users ON users.id = absensi.user_id;
```

### Pengertian tambahan

Query `JOIN` digunakan untuk menggabungkan data dari dua tabel yang saling berhubungan.

---

## 14. Contoh Struktur Database di Laravel

Dalam Laravel, struktur database biasanya dibuat menggunakan **migration**.

### Contoh migration

```php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('nama');
    $table->string('email');
    $table->timestamps();
});
```

### Pengertian tambahan

Migration membantu developer membuat struktur database melalui kode sehingga:

- mudah dilacak,
- mudah dibagikan ke tim,
- mudah dijalankan ulang,
- lebih konsisten antar lingkungan kerja.

---

## 15. Tools Database

Beberapa tools yang sering digunakan untuk mengelola database:

- phpMyAdmin
- DBeaver
- TablePlus
- MySQL Workbench
- Adminer

### Fungsi tools ini

- melihat tabel,
- menjalankan query SQL,
- membuat database,
- mengedit data,
- memantau struktur database.

---

## 16. Istilah Penting dalam Database

Berikut beberapa istilah penting yang sering muncul:

### `Query`

Perintah untuk mengambil atau mengolah data di database.

### `Schema`

Struktur database atau struktur tabel.

### `Migration`

File untuk membuat atau mengubah struktur database melalui kode.

### `Seeder`

File yang digunakan untuk mengisi data awal atau data dummy.

### `Backup`

Salinan data database untuk mencegah kehilangan data.

### `Restore`

Proses mengembalikan database dari file backup.

---

## 17. Best Practice Dasar Database

Beberapa kebiasaan baik saat merancang database:

- gunakan nama tabel yang jelas,
- gunakan primary key di setiap tabel,
- gunakan foreign key untuk relasi,
- hindari menyimpan data berulang,
- pilih tipe data yang sesuai,
- buat index pada kolom yang sering dicari,
- lakukan backup secara berkala.

### Contoh

Lebih baik menggunakan:

- `users`
- `products`
- `orders`

Daripada nama yang tidak jelas seperti:

- `data1`
- `tblbaru`
- `isi`

---

## 18. Kesimpulan

Database adalah komponen penting dalam hampir semua aplikasi modern karena digunakan untuk menyimpan, mengelola, dan menghubungkan data.

### Inti yang perlu dipahami

- database menyimpan data secara terstruktur,
- tabel terdiri dari kolom dan baris,
- primary key adalah identitas unik data,
- foreign key menghubungkan antar tabel,
- SQL digunakan untuk mengelola struktur dan isi data,
- relasi dan normalisasi membuat data lebih rapi,
- index membantu mempercepat pencarian.

### Ringkasan singkat

Jika ingin menjadi developer yang baik, memahami database adalah hal wajib. Semakin paham struktur data dan relasinya, semakin mudah membangun aplikasi yang rapi, efisien, dan scalable.
