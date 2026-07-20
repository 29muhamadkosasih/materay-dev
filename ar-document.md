BUSINESS REQUIREMENT DOCUMENT (BRD)

Sistem Arsip Dokumentasi Dokumen

1. Latar Belakang

Perusahaan membutuhkan sistem untuk mengelola arsip dokumen secara terstruktur agar proses penyimpanan, pencarian, peminjaman, dan pengembalian dokumen dapat dilakukan dengan cepat, akurat, dan terdokumentasi. Saat ini lokasi penyimpanan dokumen masih sulit dilacak sehingga diperlukan sistem yang mampu merekam posisi fisik setiap dokumen.

2. Tujuan

- Mempermudah pencatatan lokasi penyimpanan dokumen.
- Mempercepat proses pencarian dokumen.
- Mengurangi risiko kehilangan dokumen.
- Menyediakan riwayat perpindahan dan peminjaman dokumen.
- Menyediakan laporan arsip secara real-time.

3. Ruang Lingkup

Sistem mencakup pengelolaan:

- Master Ruangan
- Master Lemari
- Master Rak
- Master Box
- Master Nomor
- Master Map
- Data Dokumen
- Peminjaman Dokumen
- Pengembalian Dokumen
- Laporan Arsip

4. Struktur Lokasi Arsip

Lokasi penyimpanan dokumen menggunakan hierarki berikut:

Ruangan → Lemari → Rak → Box → Nomor → Map → Dokumen

Contoh lokasi:

Ruangan A → Lemari 1 → Rak 2 → Box 5 → Nomor 08 → Map 003

5. Functional Requirement

Master Ruangan

- Menambah ruangan.
- Mengubah data ruangan.
- Menghapus ruangan.
- Melihat daftar ruangan.

Master Lemari

- Lemari berada di dalam satu ruangan.
- Satu ruangan memiliki banyak lemari.

Master Rak

- Rak berada pada satu lemari.
- Satu lemari memiliki banyak rak.

Master Box

- Box berada pada satu rak.
- Satu rak memiliki banyak box.

Master Nomor

- Nomor berada pada satu box.
- Digunakan sebagai nomor urut penyimpanan.

Master Map

- Map berada pada satu nomor.
- Digunakan sebagai tempat penyimpanan dokumen fisik.

Master Dokumen

Data yang disimpan antara lain:

- Nomor Dokumen
- Nama Dokumen
- Jenis Dokumen
- Kategori
- Tanggal Dokumen
- Pemilik Dokumen
- Status Dokumen
- Lokasi Arsip
- File Digital (opsional)

Pencarian Dokumen

Pengguna dapat mencari berdasarkan:

- Nomor Dokumen
- Nama Dokumen
- Jenis Dokumen
- Ruangan
- Lemari
- Rak
- Box
- Nomor
- Map

Peminjaman Dokumen

Sistem mencatat:

- Peminjam
- Tanggal Pinjam
- Estimasi Kembali
- Status Pinjam

Pengembalian Dokumen

Sistem mencatat:

- Tanggal Kembali
- Kondisi Dokumen
- Status Selesai

Laporan

- Laporan seluruh dokumen.
- Laporan berdasarkan lokasi.
- Laporan dokumen dipinjam.
- Laporan dokumen kembali.
- Laporan jumlah dokumen per ruangan.

6. Non Functional Requirement

- Sistem berbasis web.
- Multi-user.
- Hak akses berdasarkan role.
- Proses pencarian cepat.
- Backup database.
- Audit log aktivitas.

7. Hak Akses

Administrator

- Mengelola seluruh master data.
- Mengelola pengguna.
- Melihat seluruh laporan.

Petugas Arsip

- Mengelola dokumen.
- Menentukan lokasi arsip.
- Mengelola peminjaman.
- Mengelola pengembalian.

User

- Melakukan pencarian dokumen.
- Mengajukan peminjaman.
- Melihat status peminjaman.

8. Alur Bisnis

1. Administrator membuat data Ruangan.
1. Administrator membuat Lemari pada setiap Ruangan.
1. Administrator membuat Rak pada setiap Lemari.
1. Administrator membuat Box pada setiap Rak.
1. Administrator membuat Nomor pada setiap Box.
1. Administrator membuat Map pada setiap Nomor.
1. Petugas memasukkan data dokumen.
1. Dokumen ditempatkan pada lokasi arsip yang dipilih.
1. Pengguna mencari dokumen melalui sistem.
1. Jika diperlukan, pengguna melakukan peminjaman.
1. Setelah selesai, dokumen dikembalikan dan status diperbarui.

1. Manfaat Sistem

- Lokasi dokumen mudah ditemukan.
- Mempercepat proses audit.
- Mengurangi kehilangan arsip.
- Monitoring dokumen lebih akurat.
- Meningkatkan efisiensi pengelolaan arsip.

---

## DESAIN SISTEM

### 10. Arsitektur Sistem

Sistem Arsip Dokumentasi dirancang dengan 3 layer utama:

1. **Client Layer (Presentation)** - Web Browser

   - Interface user-friendly
   - Responsive design
   - Real-time data updates

2. **Application Layer (Business Logic)** - Laravel Framework

   - Authentication & Authorization
   - Master Data Management
   - Document Management
   - Loan Management
   - Report Generator
   - Audit Logging

3. **Data Layer** - MySQL Database + File Storage
   - Master data storage
   - Transactional data
   - Digital document files
   - Audit logs

### 11. Database Schema - Entity Relationship

**Tabel Master:**

- `rooms` - Ruangan penyimpanan
- `cabinets` - Lemari di ruangan
- `shelves` - Rak di lemari
- `boxes` - Box di rak
- `numbers` - Nomor di box
- `folders` - Map/folder di nomor
- `document_types` - Jenis dokumen
- `document_categories` - Kategori dokumen

**Tabel Data:**

- `documents` - Data dokumen dengan metadata
- `users` - User sistem
- `loan_history` - Riwayat peminjaman

**Tabel Sistem:**

- `roles` - Role pengguna
- `permissions` - Permission setiap role
- `audit_logs` - Log aktivitas pengguna

**Relasi Utama:**

```
Room (1) ──→ (N) Cabinet ──→ (N) Shelf ──→ (N) Box ──→ (N) Number ──→ (N) Folder ──→ (N) Document
```

### 12. Alur Program Secara Umum

**FASE 1: Setup Awal (Admin)**

1. Admin membuat master Ruangan
2. Admin membuat Lemari di setiap Ruangan
3. Admin membuat Rak di setiap Lemari
4. Admin membuat Box di setiap Rak
5. Admin membuat Nomor di setiap Box
6. Admin membuat Map/Folder di setiap Nomor

**FASE 2: Operasional Harian (Petugas Arsip)** 7. Petugas input data dokumen (nomor, nama, jenis, kategori, pemilik, upload file) 8. Petugas tentukan lokasi arsip fisik untuk dokumen 9. Sistem update status dokumen menjadi TERSEDIA

**FASE 3: Pencarian & Peminjaman (User)** 10. User login ke sistem 11. User cari dokumen (berdasarkan nomor, nama, jenis, kategori, atau lokasi) 12. User lihat detail dokumen termasuk lokasi arsip-nya 13. Jika dokumen tersedia, user klik "Pinjam" dan isi form permohonan 14. Sistem save permohonan dengan status PENDING

**FASE 4: Approval Peminjaman (Petugas Arsip)** 15. Petugas review permohonan peminjaman 16. Petugas klik APPROVE atau REJECT 17. Jika APPROVE: - Sistem update status peminjaman = APPROVED - Sistem update status dokumen = LOANED_OUT - Notifikasi dikirim ke peminjam 18. Jika REJECT: Notifikasi penolakan dikirim ke peminjam

**FASE 5: Pengambilan & Penggunaan** 19. User ambil dokumen di lokasi arsip yang ditentukan 20. User gunakan dokumen sesuai kebutuhan

**FASE 6: Pengembalian (Petugas Arsip)** 21. Petugas akses menu pengembalian dokumen 22. Petugas cari dan pilih peminjaman yang aktif 23. Petugas isi form pengembalian: - Tanggal kembali - Kondisi dokumen (baik/rusak ringan/rusak berat) 24. Sistem hitung keterlambatan dan denda otomatis 25. Sistem update status peminjaman = COMPLETED 26. Sistem update status dokumen = TERSEDIA 27. Jika rusak atau terlambat, notifikasi dikirim ke peminjam

**FASE 7: Laporan & Monitoring** 28. Admin/Petugas dapat membuat berbagai laporan: - Laporan seluruh dokumen - Laporan berdasarkan lokasi - Laporan dokumen dipinjam - Laporan dokumen terlambat - Laporan jumlah dokumen per ruangan

### 13. Flow Peminjaman Dokumen (Detail)

```
User Ingin Pinjam
    ↓
[Cari Dokumen] - Filter: nomor, nama, jenis, kategori, lokasi
    ↓
Hasil Pencarian Ditampilkan
    ↓
Dokumen Ditemukan? ────→ Tidak → Notifikasi "Tidak Ada Hasil"
    ↓ Ya
[Lihat Detail Dokumen]
Periksa Status Ketersediaan
    ↓
Tersedia? ────→ Tidak (Sedang Dipinjam) → Tampilkan "Estimasi Kembali" → Kembali ke Cari
    ↓ Ya
[Klik Tombol "Pinjam"]
    ↓
[Isi Form Peminjaman]
- Nama Peminjam (auto)
- Email (auto)
- Tanggal Pinjam (hari ini)
- Estimasi Kembali (user pilih)
- Keperluan (user isi)
    ↓
[Kirim Permohonan]
    ↓
Sistem: Simpan ke DB (Status = PENDING)
    ↓
Notifikasi ke Petugas Arsip
    ↓
⏳ Tunggu Persetujuan Petugas
    ↓
Petugas Review & Decide
    ↓
REJECTED ──→ Kirim Notifikasi Penolakan
    ↓ APPROVED
Sistem Update: Status = APPROVED
    ↓
Kirim Notifikasi Sukses ke Peminjam
(Nomor Referensi, Lokasi Pengambilan, Tanggal Harus Kembali)
    ↓
User Ambil Dokumen di Lokasi Arsip
    ↓
✅ PEMINJAMAN AKTIF
```

### 14. Flow Pengembalian Dokumen (Detail)

```
Petugas Akses Menu Pengembalian
    ↓
[Cari Peminjaman Aktif]
Filter: Nomor Referensi, Nama Peminjam, atau Status = APPROVED
    ↓
Tampilkan Daftar Peminjaman Aktif
    ↓
[Pilih Peminjaman]
    ↓
[Lihat Detail]
- Dokumen yang Dipinjam
- Tanggal Pinjam
- Tanggal Harus Kembali
- Status Peminjaman
    ↓
[Isi Form Pengembalian]
- Tanggal Kembali (hari ini)
- Kondisi: Baik / Rusak Ringan / Rusak Berat
- Catatan (opsional)
    ↓
[Simpan Pengembalian]
    ↓
Sistem Hitung Keterlambatan
    ↓
Terlambat? ────→ Ya → Hitung Denda (Hari Terlambat × Tarif Harian)
    ↓ Tidak         ↓
    └──────────→ Catat Denda di DB
                 Kirim Notifikasi Denda
                 ↓
Sistem Cek Kondisi Dokumen
    ↓
├─ Baik ──────→ Update Status Peminjaman = COMPLETED
├─ Rusak Ringan → Catat Biaya Perbaikan
└─ Rusak Berat → Hitung Ganti Rugi
    ↓
Update Status Dokumen = TERSEDIA
    ↓
Kirim Notifikasi Pengembalian Sukses
    ↓
Arsipkan ke Loan History
    ↓
✅ PENGEMBALIAN SELESAI
```

### 15. Hak Akses Berdasarkan Role

**ADMINISTRATOR**

- Kelola Master Data (Ruangan, Lemari, Rak, Box, Nomor, Map)
- Kelola User (Create, Edit, Delete)
- Kelola Role & Permission
- View Semua Laporan
- System Configuration
- Backup Database
- View Audit Log

**PETUGAS ARSIP**

- Input Data Dokumen
- Tentukan Lokasi Arsip Dokumen
- Update Status Dokumen
- Manage Peminjaman (Approve/Reject)
- Catat Pengembalian Dokumen
- Hitung Denda Otomatis
- Laporan Operasional (Dokumen, Peminjaman, Pengembalian)
- View Audit Log

**USER/PEMINJAM**

- Search Dokumen (Filter: nomor, nama, jenis, kategori, lokasi)
- Lihat Detail Dokumen
- Ajukan Peminjaman
- Lihat Status Peminjaman
- Lihat Riwayat Peminjaman
- Lihat Denda (jika ada)

### 16. Tech Stack & Library

**Backend:**

- Laravel Framework 11+
- PHP 8.0+
- MySQL 5.7+ / MariaDB

**Frontend:**

- Blade Template
- Bootstrap 5
- jQuery/AJAX

**Library Composer:**

- `spatie/laravel-permission` - RBAC (Role Based Access Control)
- `yajra/laravel-datatables-oracle` - Server-side DataTables
- `maatwebsite/excel` - Import/Export Excel
- `barryvdh/laravel-dompdf` - Generate PDF

### 17. Status & Enum

**Status Dokumen:**

- `AVAILABLE` - Dokumen tersedia untuk dipinjam
- `LOANED_OUT` - Sedang dipinjam
- `ARCHIVED` - Diarsipkan
- `DAMAGED` - Rusak tidak bisa dipinjam

**Status Peminjaman:**

- `PENDING` - Permohonan menunggu persetujuan
- `APPROVED` - Permohonan disetujui, siap diambil
- `ACTIVE` - Peminjaman sedang berjalan
- `OVERDUE` - Peminjaman terlambat
- `COMPLETED` - Pengembalian selesai
- `REJECTED` - Permohonan ditolak

**Kondisi Dokumen (saat pengembalian):**

- `BAIK` - Kondisi baik, tidak ada kerusakan
- `RUSAK_RINGAN` - Ada kerusakan kecil, masih bisa dipinjam lagi
- `RUSAK_BERAT` - Rusak parah, tidak bisa dipinjam, perlu penggantian

### 18. Fitur Utama

1. **Authentication & Authorization**

   - Login dengan email & password
   - Register user baru (pending approval)
   - Session management
   - Role-based access control

2. **Master Data Management**

   - CRUD Ruangan, Lemari, Rak, Box, Nomor, Map
   - Validasi hierarki lokasi
   - Soft delete untuk audit trail

3. **Document Management**

   - Input dokumen dengan metadata lengkap
   - Upload file digital (PDF, DOC, XLS, JP, PNG, dst)
   - Tentukan lokasi arsip fisik (Ruangan → Map)
   - Search dengan multiple filter
   - Update/Delete dokumen
   - View digital file

4. **Loan Management**

   - User ajukan permohonan peminjaman
   - Petugas approve/reject permohonan
   - Track status peminjaman real-time
   - Auto-calculate denda keterlambatan
   - Catat kondisi dokumen saat kembali
   - Generate bukti pengembalian

5. **Report & Analytics**

   - Laporan seluruh dokumen
   - Laporan berdasarkan lokasi/ruangan
   - Laporan dokumen dipinjam/overdue
   - Dashboard dengan statistik & chart
   - Export ke PDF/Excel

6. **System Administration**
   - User management (create, edit, delete, activate/deactivate)
   - Role & Permission management
   - System settings (nama perusahaan, logo, tarif denda)
   - Database backup & restore
   - Audit log viewer

### 19. Skenario Penggunaan (Use Case)

**Use Case 1: Admin Setup Awal**

- Admin login
- Admin buat Ruangan "Ruang Filing Utama"
- Admin buat 3 Lemari di Ruangan tsb
- Admin buat Rak di setiap Lemari
- Admin buat Box di setiap Rak
- Admin buat Nomor & Map di setiap Box
- Setup siap untuk operasional

**Use Case 2: Petugas Input Dokumen**

- Petugas login
- Petugas buka menu "Input Dokumen"
- Petugas isi form: Nomor Dokumen, Nama, Jenis, Kategori, Pemilik, Upload File
- Petugas tentukan lokasi: Ruangan A → Lemari 1 → Rak 2 → Box 5 → Nomor 08 → Map 003
- Petugas klik Simpan
- Sistem update status dokumen = AVAILABLE

**Use Case 3: User Cari & Pinjam Dokumen**

- User login
- User klik "Cari Dokumen"
- User cari dengan filter: Nama = "Kontrak 2024", Jenis = "Kontrak"
- Sistem tampilkan hasil dengan lokasi arsip-nya
- User klik detail dokumen → Lihat lokasi di Ruangan A, Lemari 1, Rak 2, dst
- User klik "Pinjam" → Isi form → Kirim permohonan
- Sistem kirim notifikasi ke Petugas

**Use Case 4: Petugas Approve Peminjaman**

- Petugas login
- Petugas lihat daftar permohonan peminjaman (Status = PENDING)
- Petugas review → Klik APPROVE
- Sistem update status = APPROVED
- Sistem kirim notifikasi ke User dengan lokasi pengambilan

**Use Case 5: Petugas Catat Pengembalian**

- User sudah selesai pakai dokumen
- Petugas buka menu "Pengembalian Dokumen"
- Petugas cari permohonan peminjaman user
- Petugas isi form: Tanggal Kembali, Kondisi = BAIK
- Sistem hitung keterlambatan (jika ada) → Denda = 0
- Sistem update status peminjaman = COMPLETED
- Sistem update status dokumen = AVAILABLE
- Sistem kirim bukti pengembalian ke User

**Use Case 6: Admin Lihat Laporan**

- Admin login
- Admin buka menu Laporan
- Admin pilih "Laporan Seluruh Dokumen"
- Sistem tampilkan tabel dengan filter options
- Admin bisa export ke PDF/Excel
- Admin bisa lihat chart statistik (jumlah dokumen per kategori, per ruangan, dst)

### 20. Timeline Development (Estimasi)

**Sprint 1: Setup & Authentication (1-2 minggu)**

- Database migration
- User model & authentication
- Role & permission setup
- Dashboard template

**Sprint 2: Master Data Management (2 minggu)**

- Room, Cabinet, Shelf, Box, Number, Folder CRUD
- Validation & hierarchy checking
- UI untuk master data

**Sprint 3: Document Management (2 minggu)**

- Document CRUD
- File upload
- Search & filter
- Document detail view

**Sprint 4: Loan Management (2-3 minggu)**

- Loan request form
- Approval system
- Return processing
- Fine calculation

**Sprint 5: Reporting & Admin (1-2 minggu)**

- Report generator
- Dashboard & charts
- System administration
- Audit log

**Sprint 6: Testing & Deployment (1 minggu)**

- Unit & integration testing
- UAT & bug fixing
- Deployment preparation

Total: **9-12 minggu** untuk development & testing

---

## DATABASE DESIGN - LENGKAP & DETAIL

### 21. Struktur Tabel Utama

#### Tabel: rooms, cabinets, shelves, boxes, numbers, folders

Hierarki lokasi: **Room ? Cabinet ? Shelf ? Box ? Number ? Folder**

Setiap level memiliki:

- {location}\_id (PK)
- parent_id (FK ke parent)
- {location}\_code (UNIQUE)
- {location}\_name
- status (ENUM: active/inactive)
- created_by (FK to users)
- Timestamps (created_at, updated_at, deleted_at)

---

#### Tabel: document_types, document_categories

Master data klasifikasi:

- id (PK)
- {type/category}\_code (UNIQUE)
- {type/category}\_name
- description
- status
- created_by

---

#### Tabel: documents (CORE)

`
Fields:

- document_number (UNIQUE)
- document_name
- document_type_id, document_category_id (FK)
- document_date (DATE)
- owner_name, owner_department
- folder_id (FK - lokasi fisik)
- file_path, file_name, file_size, file_type
- status: AVAILABLE / LOANED_OUT / ARCHIVED / DAMAGED
- retention_year, is_important, total_copies
- created_by, updated_at, deleted_at
  `

---

#### Tabel: loan_history (CORE)

`
Fields:

- loan_number (UNIQUE)
- document_id (FK)
- user_id (FK - peminjam)
- loan_date, estimated_return_date, actual_return_date
- status: PENDING ? APPROVED ? ACTIVE ? COMPLETED/REJECTED/OVERDUE
- reason_loan (TEXT)
- approval_by (FK), approval_date
- condition_on_return: BAIK / RUSAK_RINGAN / RUSAK_BERAT
- fine_amount, fine_reason
- returned_by (FK), returned_date
  `

---

#### Tabel: audit_logs, fine_rates, system_settings

- **audit_logs**: user_id, activity, table_name, record_id, old_value, new_value
- **fine_rates**: rate_type (OVERDUE/DAMAGE_LIGHT/DAMAGE_HEAVY), amount_per_day, max_amount, status
- **system_settings**: setting_key, setting_value, setting_type (string/number/boolean/json)

---

### 22. Laravel Models - Core Relationships

**Document Model:**
`php
class Document extends Model {
public function documentType() { return \->belongsTo(DocumentType::class); }
public function category() { return \->belongsTo(DocumentCategory::class); }
public function folder() { return \->belongsTo(Folder::class); }
public function loanHistories() { return \->hasMany(LoanHistory::class); }
public function activeLoan() { return \->hasOne(LoanHistory::class)
->where('status', '!=', 'COMPLETED'); }

    public function scopeAvailable(\) { return \->where('status', 'AVAILABLE'); }
    public function canBeBorrowed() { return \->status === 'AVAILABLE'; }
    public function getLocationPath() { return \->folder->getFullLocation(); }

}
`

**LoanHistory Model:**
`php
class LoanHistory extends Model {
public function document() { return \->belongsTo(Document::class); }
public function user() { return \->belongsTo(User::class, 'user_id'); }
public function approvedBy() { return \->belongsTo(User::class, 'approval_by'); }
public function returnedBy() { return \->belongsTo(User::class, 'returned_by'); }

    public function scopeOverdue(\) { return \->whereIn('status', ['ACTIVE', 'OVERDUE'])
        ->where('estimated_return_date', '<', now()->toDateString()); }

    public function isOverdue() {
        return !in_array(\->status, ['COMPLETED', 'REJECTED']) &&
               now()->toDateString() > \->estimated_return_date;
    }

    public function calculateFine() {
        if (!\->isOverdue()) return 0;
        \ = FineRate::where('rate_type', 'OVERDUE')->where('status', 'active')->first();
        \ = \->estimated_return_date->diffInDays(now());
        return \ ? min(\ * \->amount_per_day, \->max_amount ?? 999999) : 0;
    }

    public function approve(\) {
        \->status = 'APPROVED';
        \->approval_by = \;
        \->approval_date = now();
        \->document->status = 'LOANED_OUT';
        \->document->save();
        return \->save();
    }

    public function recordReturn(\, \, \ = null) {
        \->status = 'COMPLETED';
        \->actual_return_date = now()->toDateString();
        \->condition_on_return = \;
        \->returned_by = \;
        if (\->isOverdue()) {
            \->fine_amount = \->calculateFine();
            \->fine_reason = 'Keterlambatan Pengembalian';
        }
        \->document->status = 'AVAILABLE';
        \->document->save();
        return \->save();
    }

}
`

**Folder Model (Location Path):**
`php
class Folder extends Model {
public function number() { return \->belongsTo(Number::class); }
public function documents() { return \->hasMany(Document::class); }

    public function getFullLocation() {
        \ = \->number; \ = \->box; \ = \->shelf;
        \ = \->cabinet; \ = \->room;
        return "\->room_code ? \->cabinet_code ? \->shelf_code ? \->box_code ? \->number_code ? \->folder_code";
    }

}
`

---

### 23. Relationships Diagram

`Room (1) --[N]? Cabinet --[N]? Shelf --[N]? Box --[N]? Number --[N]? Folder --[N]? Document
                                                                                        ?
DocumentType (1) --[N]? Document                                               LoanHistory
DocumentCategory (1) --[N]? Document                                                 ?
                                                                               User (peminjam)
User (1) --[N]? AuditLog`

---

### 24. Query Contoh

**Cari dokumen di ruangan tertentu:**
\\\php
\ = Document::whereHas('folder.number.box.shelf.cabinet',
fn(\) => \->where('room_id', 1)
)->get();

// Lihat lokasi lengkap
foreach(\ as \) {
echo \->folder->getFullLocation(); // R001 ? L01 ? R01 ? B01 ? N01 ? F001
}
\\\

**Manajemen peminjaman:**
\\\php
// Buat permohonan
\ = LoanHistory::create([
'loan_number' => 'LOAN-'.date('YmdHis'),
'document_id' => 5,
'user_id' => 3,
'loan_date' => now(),
'estimated_return_date' => now()->addDays(7),
'reason_loan' => 'Keperluan kerja',
'status' => 'PENDING'
]);

// Approve (oleh staff)
\->approve(\);

// Catat pengembalian
\->recordReturn('BAIK', \);

// Cek terlambat
\ = LoanHistory::overdue()->with(['document', 'user'])->get();

// Laporan per kategori
\ = Document::selectRaw('document_category_id, COUNT(\*) as total')
->groupBy('document_category_id')
->with('category')
->get();
\\\

Database design Anda sekarang **100% LENGKAP** dengan struktur tabel, models, relasi, dan contoh queries! ??

---

## DAFTAR LENGKAP TABEL YANG DIBUTUHKAN

### Total: 18 Tabel

#### KATEGORI 1: AUTHENTICATION & MANAJEMEN USER (5 tabel)

| No  | Tabel                 | Fungsi                             |
| --- | --------------------- | ---------------------------------- |
| 1   | users                 | Penyimpanan data pengguna (login)  |
| 2   | roles                 | Definisi role (admin, staff, user) |
| 3   | permissions           | Definisi permission/hak akses      |
| 4   | model_has_roles       | Relasi user ke role                |
| 5   | model_has_permissions | Relasi user ke permission          |

---

#### KATEGORI 2: LOKASI HIERARKI - LEVEL PENYIMPANAN FISIK (6 tabel)

| No  | Tabel    | Fungsi                      | Hierarki                 |
| --- | -------- | --------------------------- | ------------------------ |
| 6   | rooms    | Ruangan penyimpanan dokumen | Level 1                  |
| 7   | cabinets | Lemari di dalam ruangan     | Level 2 (FK: room_id)    |
| 8   | shelves  | Rak di dalam lemari         | Level 3 (FK: cabinet_id) |
| 9   | boxes    | Box di dalam rak            | Level 4 (FK: shelf_id)   |
| 10  | numbers  | Nomor/slot di dalam box     | Level 5 (FK: box_id)     |
| 11  | folders  | Map/folder di dalam nomor   | Level 6 (FK: number_id)  |

**Alur Hierarki:**
Room ? Cabinet ? Shelf ? Box ? Number ? Folder
(Contoh: R001 ? L01 ? R01 ? B01 ? N01 ? F001)

---

#### KATEGORI 3: MASTER DATA DOKUMEN (2 tabel)

| No  | Tabel               | Fungsi                                                                 |
| --- | ------------------- | ---------------------------------------------------------------------- |
| 12  | document_types      | Master jenis dokumen (Kontrak, Invoice, Surat, Laporan, Proposal, dll) |
| 13  | document_categories | Master kategori dokumen (Keuangan, HR, Legal, Operasional, dll)        |

---

#### KATEGORI 4: DATA UTAMA - DOKUMEN & PEMINJAMAN (2 tabel)

| No  | Tabel        | Fungsi                                                             | Keys                                  |
| --- | ------------ | ------------------------------------------------------------------ | ------------------------------------- |
| 14  | documents    | **DATA DOKUMEN UTAMA** (metadata lengkap + lokasi fisik)           | id, document_number, folder_id        |
| 15  | loan_history | **RIWAYAT PEMINJAMAN & PENGEMBALIAN** (tracking setiap peminjaman) | id, loan_number, document_id, user_id |

---

#### KATEGORI 5: KONFIGURASI & TARIF DENDA (2 tabel)

| No  | Tabel           | Fungsi                                                 |
| --- | --------------- | ------------------------------------------------------ |
| 16  | fine_rates      | Tarif denda (OVERDUE, DAMAGE_LIGHT, DAMAGE_HEAVY)      |
| 17  | system_settings | Konfigurasi global (nama perusahaan, logo, tarif, dll) |

---

#### KATEGORI 6: AUDIT & LOGGING (1 tabel)

| No  | Tabel      | Fungsi                                                                  |
| --- | ---------- | ----------------------------------------------------------------------- |
| 18  | audit_logs | Tracking aktivitas user (CREATE, UPDATE, DELETE pada tabel konkritikal) |

---

### RINGKASAN STRUKTUR TABEL

\\\
AUTHENTICATION (5 tabel)
+- users
+- roles
+- permissions
+- model_has_roles
+- model_has_permissions

LOKASI HIERARKI (6 tabel)
+- rooms
+- cabinets
+- shelves
+- boxes
+- numbers
+- folders

MASTER DATA (2 tabel)
+- document_types
+- document_categories

DATA UTAMA (2 tabel)
+- documents (? CORE)
+- loan_history (? CORE)

SUPPORT (3 tabel)
+- fine_rates
+- system_settings
+- audit_logs

TOTAL: 18 TABEL
\\\

---

### PRIORITAS TABEL (BY IMPORTANCE)

**CRITICAL (?? Wajib Ada):**

- documents ? Data dokumen (jantung sistem)
- loan_history ? Riwayat peminjaman (audit trail)
- rooms, cabinets, shelves, boxes, numbers, folders ? Hierarki lokasi (6 tabel)
- users ? User/login (autentikasi)

**IMPORTANT (?? Sangat Penting):**

- document_types ? Klasifikasi dokumen
- document_categories ? Kategori dokumen
- roles, permissions, model_has_roles ? RBAC

**SUPPORT (?? Pendukung):**

- fine_rates ? Perhitungan denda
- system_settings ? Konfigurasi sistem
- audit_logs ? Tracking aktivitas

---

### DEPENDENCY TABEL (URUTAN PEMBUATAN DATABASE)

Rekomendasi urutan creating migrations:

1. users (independent)
2. roles, permissions (independent)
3. model_has_roles, model_has_permissions (dependent on users, roles, permissions)
4. rooms (independent)
5. cabinets (FK: rooms)
6. shelves (FK: cabinets)
7. boxes (FK: shelves)
8. numbers (FK: boxes)
9. folders (FK: numbers)
10. document_types (independent)
11. document_categories (independent)
12. documents (FK: document_types, document_categories, folders)
13. loan_history (FK: documents, users)
14. fine_rates (independent)
15. system_settings (independent)
16. audit_logs (FK: users)

---

### PENJELASAN TABEL KRITIS

#### Tabel: documents (TABEL INTI)

Menyimpan data dokumen dengan metadata lengkap:

- document_number (UNIQUE - nomor referensi)
- document_name (nama dokumen)
- document_type_id, document_category_id (klasifikasi)
- document_date (tanggal dokumen)
- owner_name, owner_department (pemilik dokumen)
- folder_id (lokasi penyimpanan fisik)
- file_path (path file digital jika ada)
- status (AVAILABLE/LOANED_OUT/ARCHIVED/DAMAGED)
- retention_year (berapa tahun disimpan)
- is_important (flag dokumen penting)

#### Tabel: loan_history (TABEL INTI)

Menyimpan riwayat setiap peminjaman dokumen:

- loan_number (UNIQUE - nomor referensi peminjaman)
- document_id (dokumen yang dipinjam)
- user_id (siapa yang meminjam)
- loan_date, estimated_return_date, actual_return_date (tanggal)
- status (PENDING ? APPROVED ? ACTIVE ? COMPLETED/OVERDUE/REJECTED)
- reason_loan (alasan peminjaman)
- approval_by, approval_date (siapa approve, kapan)
- condition_on_return (BAIK/RUSAK_RINGAN/RUSAK_BERAT)
- fine_amount, fine_reason (denda otomatis)
- returned_by, returned_date (siapa catat pengembalian, kapan)

#### Tabel: Hierarki Lokasi (6 tabel)

Struktur pohon untuk lokasi fisik penyimpanan:

- Room ? Cabinet ? Shelf ? Box ? Number ? Folder
- Setiap level memiliki: id, parent_id (FK), code (UNIQUE), name, status
- Memungkinkan tracking lokasi dokumen secara presisi

---

### MIGRATION NAMING CONVENTION

Saat membuat migration files, gunakan nama:

1. 2024_01_01_000001_create_users_table
2. 2024_01_01_000002_create_roles_table
3. 2024_01_01_000003_create_permissions_table
4. 2024_01_01_000004_create_rooms_table
5. 2024_01_01_000005_create_cabinets_table
6. 2024_01_01_000006_create_shelves_table
7. 2024_01_01_000007_create_boxes_table
8. 2024_01_01_000008_create_numbers_table
9. 2024_01_01_000009_create_folders_table
10. 2024_01_01_000010_create_document_types_table
11. 2024_01_01_000011_create_document_categories_table
12. 2024_01_01_000012_create_documents_table
13. 2024_01_01_000013_create_loan_history_table
14. 2024_01_01_000014_create_fine_rates_table
15. 2024_01_01_000015_create_system_settings_table
16. 2024_01_01_000016_create_audit_logs_table
17. 2024_01_01_000017_create_model_has_roles_table
18. 2024_01_01_000018_create_model_has_permissions_table

---

### RELASI ANTAR TABEL OVERVIEW

\\\
User (1) --[N]-? LoanHistory
----[N]--? AuditLog
----[N]--? created_by (Rooms, Cabinets, etc)

DocumentType (1) --[N]-? Document
DocumentCategory (1) --[N]-? Document

Room (1) --[N]-? Cabinet --[N]-? Shelf --[N]-? Box --[N]-? Number --[N]-? Folder --[N]-? Document

Document (1) --[N]-? LoanHistory
+- user_id (peminjam)
+- approval_by (staff yang approve)
+- returned_by (staff yang catat return)
\\\

## Sistem dokumentasi tabel sudah **100% LENGKAP** dengan kategorisasi, prioritas, dependency, dan penjelasan detail! ??

## 26. Struktur Kolom Setiap Tabel

### Authentication & User Management

**users**

- id
- name
- email
- email_verified_at
- password
- phone
- role
- status
- remember_token
- created_by
- created_at
- updated_at
- deleted_at

**roles**

- id
- name
- guard_name
- created_at
- updated_at

**permissions**

- id
- name
- guard_name
- created_at
- updated_at

**model_has_roles**

- role_id
- model_type
- model_id

**model_has_permissions**

- permission_id
- model_type
- model_id

### Lokasi Hierarki

**rooms**

- id
- room_code
- room_name
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

**cabinets**

- id
- room_id
- cabinet_code
- cabinet_name
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

**shelves**

- id
- cabinet_id
- shelf_code
- shelf_name
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

**boxes**

- id
- shelf_id
- box_code
- box_name
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

**numbers**

- id
- box_id
- number_code
- number_name
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

**folders**

- id
- number_id
- folder_code
- folder_name
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

### Master Data Dokumen

**document_types**

- id
- type_code
- type_name
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

**document_categories**

- id
- category_code
- category_name
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

### Data Utama

**documents**

- id
- document_number
- document_name
- document_type_id
- document_category_id
- document_date
- owner_name
- owner_department
- folder_id
- file_path
- file_name
- file_size
- file_type
- status
- retention_year
- is_important
- total_copies
- notes
- created_by
- created_at
- updated_at
- deleted_at

**loan_history**

- id
- loan_number
- document_id
- user_id
- loan_date
- estimated_return_date
- actual_return_date
- status
- reason_loan
- approval_by
- approval_date
- condition_on_return
- fine_amount
- fine_reason
- returned_by
- returned_date
- notes
- created_at
- updated_at
- deleted_at

### Konfigurasi & Tarif Denda

**fine_rates**

- id
- rate_type
- amount_per_day
- max_amount
- description
- status
- created_by
- created_at
- updated_at
- deleted_at

**system_settings**

- id
- setting_key
- setting_value
- setting_type
- description
- status
- created_at
- updated_at

### Audit & Logging

**audit_logs**

- id
- user_id
- activity
- table_name
- record_id
- old_value
- new_value
- ip_address
- user_agent
- created_at

## 27. Desain Database dan Relasi

### 27.1. Struktur Database Secara Umum

Database dibagi menjadi 4 area utama:

1. Authentication & Authorization
   - `users`, `roles`, `permissions`, `model_has_roles`, `model_has_permissions`
2. Lokasi Hierarki Fisik
   - `rooms`, `cabinets`, `shelves`, `boxes`, `numbers`, `folders`
3. Master Data Dokumen
   - `document_types`, `document_categories`
4. Data Operasional
   - `documents`, `loan_history`, `fine_rates`, `system_settings`, `audit_logs`

### 27.2. Relasi Utama

- `rooms` 1 → N `cabinets`
- `cabinets` 1 → N `shelves`
- `shelves` 1 → N `boxes`
- `boxes` 1 → N `numbers`
- `numbers` 1 → N `folders`
- `folders` 1 → N `documents`
- `document_types` 1 → N `documents`
- `document_categories` 1 → N `documents`
- `documents` 1 → N `loan_history`
- `users` 1 → N `loan_history` (`user_id` sebagai peminjam)
- `users` 1 → N `loan_history` (`approval_by` sebagai petugas approval)
- `users` 1 → N `loan_history` (`returned_by` sebagai petugas return)
- `users` 1 → N `audit_logs`

### 27.3. Detail Relasi dan Foreign Key

**rooms**

- `id`
- `room_code`
- `room_name`
- relasi ke `cabinets.room_id`

**cabinets**

- `room_id` ? FK `rooms.id`
- `cabinet_code`
- `cabinet_name`
- relasi ke `shelves.cabinet_id`

**shelves**

- `cabinet_id` ? FK `cabinets.id`
- `shelf_code`
- `shelf_name`
- relasi ke `boxes.shelf_id`

**boxes**

- `shelf_id` ? FK `shelves.id`
- `box_code`
- `box_name`
- relasi ke `numbers.box_id`

**numbers**

- `box_id` ? FK `boxes.id`
- `number_code`
- `number_name`
- relasi ke `folders.number_id`

**folders**

- `number_id` ? FK `numbers.id`
- `folder_code`
- `folder_name`
- relasi ke `documents.folder_id`

**document_types**

- `type_code`
- `type_name`
- relasi ke `documents.document_type_id`

**document_categories**

- `category_code`
- `category_name`
- relasi ke `documents.document_category_id`

**documents**

- `folder_id` ? FK `folders.id`
- `document_type_id` ? FK `document_types.id`
- `document_category_id` ? FK `document_categories.id`
- relasi ke `loan_history.document_id`

**loan_history**

- `document_id` ? FK `documents.id`
- `user_id` ? FK `users.id` (peminjam)
- `approval_by` ? FK `users.id` (petugas approval)
- `returned_by` ? FK `users.id` (petugas pencatat pengembalian)

**audit_logs**

- `user_id` ? FK `users.id`
- mencatat perubahan pada tabel penting dan operasi CRUD

### 27.4. Diagram Relasi Sederhana

```
rooms -> cabinets -> shelves -> boxes -> numbers -> folders -> documents -> loan_history
document_types -> documents
document_categories -> documents
users -> loan_history
users -> audit_logs
```

### 27.5. Keterangan Relasi

- Lokasi fisik menggunakan pohon 1 ke banyak dari `rooms` sampai `folders`.
- Setiap `document` disimpan di satu `folder`.
- `loan_history` menyimpan semua event peminjaman dan pengembalian.
- `approval_by` dan `returned_by` mereferensikan `users` untuk menelusuri petugas.
- `document_types` dan `document_categories` adalah master klasifikasi dokumen.

### 27.6. Contoh FK dan Index yang Direkomendasikan

- `cabinets.room_id` INDEX + FK -> `rooms.id`
- `shelves.cabinet_id` INDEX + FK -> `cabinets.id`
- `boxes.shelf_id` INDEX + FK -> `shelves.id`
- `numbers.box_id` INDEX + FK -> `boxes.id`
- `folders.number_id` INDEX + FK -> `numbers.id`
- `documents.folder_id` INDEX + FK -> `folders.id`
- `documents.document_type_id` INDEX + FK -> `document_types.id`
- `documents.document_category_id` INDEX + FK -> `document_categories.id`
- `loan_history.document_id` INDEX + FK -> `documents.id`
- `loan_history.user_id` INDEX + FK -> `users.id`
- `loan_history.approval_by` INDEX + FK -> `users.id`
- `loan_history.returned_by` INDEX + FK -> `users.id`
- `audit_logs.user_id` INDEX + FK -> `users.id`

### 27.7. Catatan Implementasi

- Gunakan `soft deletes` pada tabel master dan dokumen untuk audit dan pemulihan.
- Gunakan `ENUM` atau `VARCHAR` untuk nilai status di `documents` dan `loan_history`.
- Gunakan `ON DELETE RESTRICT` atau `ON DELETE SET NULL` agar histori peminjaman tetap terjaga.

---
Credit: Muhamad Kosasih 2026

## 28. Implementasi dengan Laravel Livewire

### 28.1. Konsep Utama

Sistem ini dapat dibangun menggunakan Laravel Livewire untuk interface interaktif tanpa JavaScript framework kompleks. Livewire cocok untuk:

- CRUD data master lokasi (ruangan / lemari / rak / box / nomor / folder)
- CRUD dokumen dan upload file
- Form peminjaman dan pengembalian dokumen
- Filter pencarian dan pagination real-time
- Validasi dan notifikasi langsung

### 28.2. Instalasi

1. Pasang Livewire:
   - `composer require livewire/livewire`
2. Tambahkan di layout Blade:
   - `@livewireStyles` di head
   - `@livewireScripts` sebelum closing `</body>`
3. Pastikan `app.blade.php` memuat `@vite(['resources/css/app.css', 'resources/js/app.js'])` dan `@livewireStyles` / `@livewireScripts`.

### 28.3. Struktur Komponen Livewire

Gunakan komponen Livewire untuk setiap modul utama:

- `Location/RoomIndex`
- `Location/CabinetIndex`
- `Location/ShelfIndex`
- `Location/BoxIndex`
- `Location/NumberIndex`
- `Location/FolderIndex`
- `Document/DocumentIndex`
- `Document/DocumentForm`
- `Loan/LoanRequest`
- `Loan/LoanApproval`
- `Loan/ReturnForm`
- `Report/DocumentReport`
- `Report/LoanReport`

### 28.4. Contoh Komponen Livewire

**DocumentIndex.php**
- menampilkan daftar dokumen
- pencarian by nomor / nama / jenis / kategori / lokasi
- pagination Livewire
- event edit / delete

**DocumentForm.php**
- form input dokumen
- `wire:model` untuk semua field
- validasi `rules`
- upload file digital dengan `WithFileUploads`
- simpan `folder_id` berdasarkan pilihan lokasi

### 28.5. Alur Komponen Livewire

1. User membuka halaman dokumen.
2. `DocumentIndex` menampilkan daftar dan filter.
3. User klik tombol tambah / edit.
4. `DocumentForm` muncul sebagai modal atau halaman.
5. Setelah submit, Livewire memvalidasi data dan menyimpan.
6. `DocumentIndex` otomatis refresh tanpa reload.

### 28.6. Kelebihan Livewire untuk Sistem Ini

- Interaksi form lebih halus
- Validasi real-time
- Update tabel tanpa refresh penuh
- Lebih cepat dikembangkan dibanding SPA penuh
- Mudah diintegrasikan dengan `spatie/laravel-permission`

### 28.7. Rekomendasi Route dan Blade

Gunakan route sederhana untuk setiap halaman modul:

- `/admin/rooms`
- `/admin/documents`
- `/admin/loans`
- `/admin/returns`
- `/admin/reports`

Setiap route kemudian memanggil View Blade yang berisi `@livewire('location.room-index')` atau `@livewire('document.document-index')`.

### 28.8. Integrasi dengan Role & Permission

Untuk kontrol akses:

- `@can('view documents')` pada blade
- `if (! auth()->user()->can('manage loans')) abort(403);` di komponen Livewire
- gunakan `middleware(['auth', 'role:admin|petugas'])` pada route grup

### 28.9. Perbaikan UX dengan Livewire

- gunakan modal Livewire untuk tambah / edit data
- gunakan `wire:loading` untuk spinner
- gunakan event Livewire untuk notifikasi toast
- gunakan `wire:click.prevent` dan `wire:submit.prevent`

### 28.10. Catatan Khusus

- Livewire 3 direkomendasikan jika menggunakan Laravel 11 terbaru.
- untuk upload file, gunakan `wire:model="file"` dan trait `WithFileUploads`.
- untuk nested select lokasi, load data dependent secara Livewire (misal `cabinets` setelah pilih `room`).

