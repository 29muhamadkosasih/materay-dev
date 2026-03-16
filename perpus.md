# Scope Project Sistem Perpustakaan (Versi Sederhana, Lebih Detail)

Dokumen ini merangkum scope, alur, struktur data, dan langkah implementasi Laravel untuk sistem perpustakaan yang tetap sederhana tetapi siap dieksekusi.

## 1) Role, Akses, dan Modul

### Role

- **Admin**: kelola user/petugas + semua data master + laporan + pengaturan denda.
- **Petugas**: kelola buku/anggota + transaksi peminjaman/pengembalian + laporan.
- **Anggota (opsional)**: lihat katalog + riwayat pinjam sendiri.

### Modul Utama

- **Auth & Role**
  - Login/logout.
  - Pembatasan menu berdasarkan role.
- **Master Data**
  - Kategori buku.
  - Buku (dengan cover image).
  - Anggota (foto opsional).
- **Sirkulasi**
  - Peminjaman (multi-buku).
  - Pengembalian + hitung denda otomatis.
- **Laporan & Export**
  - Export Excel & PDF untuk buku, anggota, peminjaman (berdasarkan periode).
  - Cetak PDF bukti peminjaman.
- **(Opsional) QR Code**
  - Untuk kartu anggota / kode peminjaman.

## 2) Alur (Flow) End-to-End

### A. Flow Master Buku (dengan Cover)

1. Petugas buka menu **Buku** lalu klik **Tambah**.
2. Isi data buku + upload cover.
3. Sistem simpan cover ke `storage/app/public/books/...`.
4. Sistem simpan path file ke `books.cover_path`.
5. List buku menampilkan thumbnail cover.

Output: data buku tersimpan, cover dapat diakses dari `public/storage/...`.

### B. Flow Peminjaman

1. Pilih anggota.
2. Tambah buku (bisa lebih dari 1 judul).
3. Sistem validasi:
   - stok tiap buku cukup,
   - anggota aktif (`is_active = true`),
   - belum melewati batas pinjaman aktif (opsional).
4. Simpan transaksi ke `loans` dan detail ke `loan_items`.
5. Kurangi `books.quantity_available` sesuai qty pinjam.

Output: transaksi berstatus `BORROWED` dan stok berkurang otomatis.

### C. Flow Pengembalian + Denda

1. Buka detail transaksi dengan status `BORROWED`.
2. Klik **Return/Kembalikan**.
3. Sistem hitung:
   - `returned_at = today`
   - `late_days = max(0, today - due_date)`
   - `fine_total = late_days × fine_per_day`
   - `status = RETURNED`
4. Sistem kembalikan stok dari item transaksi.

Output: transaksi selesai, denda tercatat, stok kembali bertambah.

### D. Flow Laporan & Export

1. User pilih filter tanggal, status, dan (opsional) anggota/petugas.
2. Sistem tampilkan rekap di layar.
3. User klik:
   - **Export Excel** (`.xlsx`)
   - **Export PDF** (`.pdf`)

Output: file laporan terunduh sesuai filter aktif.

## 3) Desain Database (Final + Field Gambar)

### Tambahan Field Image

#### Tabel `books`

- `cover_path` (`string`, nullable) → path file cover.
- (opsional) `cover_original_name` (`string`, nullable).
- (opsional) `cover_mime` (`string`, nullable).
- (opsional) `cover_size` (`integer`, nullable).

Minimal implementasi cukup `cover_path`.

#### Tabel `members`

- `photo_path` (`string`, nullable).

### Skema Tabel Ringkas

#### `categories`

- `id`, `name`, `timestamps`.

#### `books`

- `id`
- `category_id` (FK)
- `isbn` (nullable)
- `title`
- `author`
- `publisher` (nullable)
- `year` (nullable)
- `rack_location` (nullable)
- `quantity_total` (int)
- `quantity_available` (int)
- `cover_path` (nullable)
- `timestamps`

#### `members`

- `id`
- `member_code` (unique)
- `name`
- `class` (nullable)
- `type` (`student`/`teacher`, optional)
- `phone`, `address` (nullable)
- `is_active` (boolean)
- `photo_path` (nullable, opsional)
- `timestamps`

#### `users`

- `id`, `name`, `email`, `password`, `role`, `timestamps`.

#### `loans`

- `id`
- `loan_code` (unique)
- `member_id` (FK)
- `user_id` (FK)
- `loaned_at`
- `due_date`
- `returned_at` (nullable)
- `status` (`BORROWED`/`RETURNED`)
- `fine_total` (int default 0)
- `timestamps`

#### `loan_items`

- `id`
- `loan_id` (FK)
- `book_id` (FK)
- `qty` (int default 1)
- `timestamps`

### Index Penting

- `books(title)` untuk pencarian cepat.
- `loans(status, loaned_at)` untuk laporan periodik.
- (disarankan) `loan_items(loan_id)` dan `loan_items(book_id)` untuk join cepat.

## 4) Rekomendasi Penyimpanan Image (Laravel Storage)

- Simpan file di `storage/app/public/books/`.
- Akses file melalui symlink `public/storage/...`.
- Jalankan:

```bash
php artisan storage:link
```

### Validasi Upload Cover (Disarankan)

- file harus image,
- ukuran maksimal 2MB,
- format: `jpg`, `jpeg`, `png`, `webp`.

Contoh rule Laravel:

- `nullable|image|mimes:jpg,jpeg,png,webp|max:2048`

## 5) Langkah Implementasi (Urutan Aman)

### Step 1 — Setup Project + Auth

- Laravel + Breeze untuk login.
- Tambahkan kolom `role` pada tabel `users`.
- Buat middleware role untuk proteksi route.

### Step 2 — Migration & Model

```bash
php artisan make:model Category -mcr
php artisan make:model Book -mcr
php artisan make:model Member -mcr
php artisan make:model Loan -m
php artisan make:model LoanItem -m
```

### Step 3 — CRUD Kategori, Buku, Anggota

- Buku wajib mendukung upload cover.
- Form create/edit punya input file cover.
- Saat store/update:
  - upload via `Storage::disk('public')->putFile('books', $file)`
  - simpan path ke `cover_path`
  - hapus cover lama saat diganti (agar storage bersih)

### Step 4 — Peminjaman/Pengembalian

- Generate `loan_code` otomatis (unik).
- Gunakan DB transaction agar update stok konsisten.
- Pastikan tidak bisa return dua kali untuk transaksi yang sama.

### Step 5 — Export Excel

Install package:

```bash
composer require maatwebsite/excel
```

Buat export class:

```bash
php artisan make:export BooksExport --model=Book
php artisan make:export LoansExport
php artisan make:export MembersExport --model=Member
```

Controller report (contoh):

- `exportBooksExcel()`
- `exportMembersExcel()`
- `exportLoansExcel(Request $request)` (filter periode/status)

### Step 6 — Export PDF (DOMPDF)

Install package:

```bash
composer require barryvdh/laravel-dompdf
```

Buat view PDF:

- `resources/views/pdf/loans-report.blade.php`
- `resources/views/pdf/loan-receipt.blade.php`

Controller (contoh):

- `exportLoansPdf(Request $request)`
- `printLoanReceiptPdf(Loan $loan)`

## 6) Halaman UI Wajib (Siap Demo/Screenshot)

- Login
- Dashboard
- Kategori (list + form)
- Buku (list dengan thumbnail cover)
- Form tambah/edit buku (upload cover)
- Anggota (list + form)
- Peminjaman (form multi-buku)
- List peminjaman (filter status/tanggal)
- Detail peminjaman (return + cetak bukti PDF)
- Laporan (filter + export Excel/PDF)

## 7) Aturan Bisnis Minimum (Agar Tidak Bolak-Balik)

Tetapkan sejak awal:

- Durasi pinjam default (mis. 7 hari).
- Denda per hari (mis. Rp1.000/hari).
- Batas maksimal buku aktif per anggota (mis. 3 buku).
- Apakah anggota dengan tunggakan denda boleh meminjam lagi.
- Apakah stok boleh `0` tetap tampil di katalog.

Dengan aturan ini, implementasi controller dan validasi menjadi konsisten dari awal.
