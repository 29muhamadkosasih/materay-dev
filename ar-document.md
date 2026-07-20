BUSINESS REQUIREMENT DOCUMENT (BRD)

Sistem Arsip Dokumentasi Dokumen

1. Latar Belakang

Perusahaan membutuhkan sistem untuk mengelola arsip dokumen secara terstruktur agar proses penyimpanan, pencarian, edit, dan penghapusan dokumen dapat dilakukan dengan cepat, akurat, dan terdokumentasi. Saat ini lokasi penyimpanan dokumen masih sulit dilacak sehingga diperlukan sistem yang mampu merekam posisi fisik setiap dokumen.

2. Tujuan

- Mempermudah pencatatan lokasi penyimpanan dokumen.
- Mempercepat proses pencarian dokumen.
- Mengurangi risiko kehilangan arsip.
- Menyediakan riwayat perubahan dan penghapusan dokumen.
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

Fungsi Dokumen

- Menambah dokumen.
- Mengubah dokumen.
- Menghapus dokumen.
- Menampilkan daftar dokumen.
- Menentukan lokasi arsip dokumen.

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

Laporan

- Laporan seluruh dokumen.
- Laporan berdasarkan lokasi.
- Laporan dokumen berdasarkan status.
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
- Mengedit dokumen.
- Menghapus dokumen.

User

- Melakukan pencarian dokumen.
- Melihat detail dokumen.

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
1. Petugas atau administrator mengedit data dokumen jika diperlukan.
1. Petugas atau administrator menghapus dokumen jika diperlukan.

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
   - Document Edit & Delete Management
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

**FASE 3: Pencarian & Operasional Dokumen (User/Petugas)** 10. User login ke sistem 11. User cari dokumen (berdasarkan nomor, nama, jenis, kategori, atau lokasi) 12. User lihat detail dokumen termasuk lokasi arsipnya 13. Jika perlu, petugas klik "Edit" untuk memperbarui metadata atau lokasi 14. Jika perlu, petugas klik "Hapus" untuk menghapus dokumen dari katalog

**FASE 4: Laporan & Monitoring** 15. Admin/Petugas dapat membuat berbagai laporan: - Laporan seluruh dokumen - Laporan berdasarkan lokasi - Laporan dokumen berdasarkan status - Laporan jumlah dokumen per ruangan

### 13. Flow Pengelolaan Dokumen (Detail)

```
User mencari dokumen
    ↓
[Cari Dokumen] - Filter: nomor, nama, jenis, kategori, lokasi
    ↓
Hasil Pencarian Ditampilkan
    ↓
Dokumen Ditemukan? ────→ Tidak → Notifikasi "Tidak Ada Hasil"
    ↓ Ya
[Lihat Detail Dokumen]
    ↓
Pilihan Aksi: Edit / Hapus
    ↓
Edit → Tampilkan Form Edit
Hapus → Tampilkan Konfirmasi Hapus
    ↓
[Simpan Perubahan atau Konfirmasi Hapus]
    ↓
Sistem: Simpan Perubahan atau Hapus Data
    ↓
Tampilkan Notifikasi Sukses
```

### 14. Hak Akses Berdasarkan Role

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
- Edit Dokumen
- Hapus Dokumen
- Laporan Operasional Dokumen
- View Audit Log

**USER**

- Search Dokumen (Filter: nomor, nama, jenis, kategori, lokasi)
- Lihat Detail Dokumen

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

- `AVAILABLE` - Dokumen tersedia
- `ARCHIVED` - Diarsipkan
- `DAMAGED` - Dokumen rusak atau tidak layak gunakan
- `DELETED` - Dokumen telah dihapus secara logis

### 18. Fitur Utama

1. **Authentication & Authorization**

   - Login dengan email & password
   - Manage user dan role
   - Session management
   - Role-based access control

2. **Master Data Management**

   - CRUD Ruangan, Lemari, Rak, Box, Nomor, Map
   - Validasi hierarki lokasi
   - Soft delete untuk audit trail

3. **Document Management**

   - Input dokumen dengan metadata lengkap
   - Upload file digital (PDF, DOC, XLS, JPG, PNG, dst)
   - Tentukan lokasi arsip fisik (Ruangan → Folder)
   - Search dengan multiple filter
   - Edit dokumen
   - Hapus dokumen
   - View digital file

4. **Report & Analytics**

   - Laporan seluruh dokumen
   - Laporan berdasarkan lokasi/ruangan
   - Laporan dokumen berdasarkan status
   - Dashboard dengan statistik & chart
   - Export ke PDF/Excel

5. **System Administration**
   - User management (create, edit, delete, activate/deactivate)
   - Role & Permission management
   - System settings (nama perusahaan, logo, informasi kantor)
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

**Use Case 3: User Cari Dokumen & Lihat Detail**

- User login
- User klik "Cari Dokumen"
- User mencari dengan filter: Nama, Jenis, Kategori, atau Lokasi
- Sistem tampilkan hasil dengan lokasi arsip-nya
- User klik detail dokumen → Lihat metadata dan lokasi fisik

**Use Case 4: Petugas Edit Dokumen**

- Petugas login
- Petugas buka daftar dokumen
- Petugas pilih dokumen yang akan diubah
- Petugas edit metadata, kategori, atau lokasi arsip
- Petugas simpan perubahan
- Sistem update data dokumen dan tampilkan notifikasi sukses

**Use Case 5: Petugas Hapus Dokumen**

- Petugas login
- Petugas cari dokumen yang tidak lagi diperlukan
- Petugas pilih aksi "Hapus"
- Sistem tampilkan konfirmasi penghapusan
- Petugas konfirmasi
- Sistem hapus dokumen secara logis dan catat audit log

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

**Sprint 4: Document Operations (2-3 minggu)**

- Document edit form
- Document delete confirmation
- Location update
- Approval tidak diperlukan untuk edit/hapus sederhana

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
- status: AVAILABLE / ARCHIVED / DAMAGED
- retention_year, is_important, total_copies
- created_by, updated_at, deleted_at
  `

---

#### Tabel: audit_logs, system_settings

- **audit_logs**: user_id, activity, table_name, record_id, old_value, new_value
- **system_settings**: setting_key, setting_value, setting_type (string/number/boolean/json)

---

### 22. Laravel Models - Core Relationships

**Document Model:**
`php
class Document extends Model {
public function documentType() { return $this->belongsTo(DocumentType::class); }
public function category() { return $this->belongsTo(DocumentCategory::class); }
public function folder() { return $this->belongsTo(Folder::class); }

    public function scopeActive($query) { return $query->where('status', '!=', 'DELETED'); }
    public function scopeSearch($query, $term) {
        return $query->where('document_number', 'like', "%{$term}%")
            ->orWhere('document_name', 'like', "%{$term}%");
    }

    public function getLocationPath() {
        return $this->folder->getFullLocation();
    }

}
`

**Folder Model (Location Path):**
`php
class Folder extends Model {
public function number() { return $this->belongsTo(Number::class); }
public function documents() { return $this->hasMany(Document::class); }

    public function getFullLocation() {
        $number = $this->number;
        $box = $number->box;
        $shelf = $box->shelf;
        $cabinet = $shelf->cabinet;
        $room = $cabinet->room;

        return "{$room->room_code} / {$cabinet->cabinet_code} / {$shelf->shelf_code} / {$box->box_code} / {$number->number_code} / {$this->folder_code}";
    }

}
`

---

### 23. Relationships Diagram

`Room (1) --[N]? Cabinet --[N]? Shelf --[N]? Box --[N]? Number --[N]? Folder --[N]? Document
DocumentType (1) --[N]? Document
DocumentCategory (1) --[N]? Document
User (1) --[N]? AuditLog`

---

### 24. Query Contoh

**Cari dokumen di ruangan tertentu:**
\\\php
$documents = Document::whereHas('folder.number.box.shelf.cabinet', function ($query) {
$query->where('room_id', 1);
})->get();

// Lihat lokasi lengkap
foreach ($documents as $document) {
echo $document->folder->getFullLocation();
}
\\\

**Contoh update dokumen:**
\\\php
$document = Document::find(5);
$document->update([
'document_name' => 'Revisi Kontrak 2026',
'folder_id' => 12,
]);
\\\

Database design Anda sekarang **100% LENGKAP** dengan struktur tabel, models, relasi, dan contoh queries! ??

---

## DAFTAR LENGKAP TABEL YANG DIBUTUHKAN

### Total: 16 Tabel

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

#### KATEGORI 4: DATA UTAMA - DOKUMEN (1 tabel)

| No  | Tabel     | Fungsi                                                   | Keys                           |
| --- | --------- | -------------------------------------------------------- | ------------------------------ |
| 14  | documents | **DATA DOKUMEN UTAMA** (metadata lengkap + lokasi fisik) | id, document_number, folder_id |

---

#### KATEGORI 5: KONFIGURASI & SISTEM (1 tabel)

| No  | Tabel           | Fungsi                                                |
| --- | --------------- | ----------------------------------------------------- |
| 15  | system_settings | Konfigurasi global (nama perusahaan, logo, informasi) |

---

#### KATEGORI 6: AUDIT & LOGGING (1 tabel)

| No  | Tabel      | Fungsi                                                              |
| --- | ---------- | ------------------------------------------------------------------- |
| 16  | audit_logs | Tracking aktivitas user (CREATE, UPDATE, DELETE pada tabel penting) |

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

DATA UTAMA (1 tabel)
+- documents (? CORE)

SUPPORT (2 tabel)
+- system_settings
+- audit_logs

TOTAL: 16 TABEL
\\\

---

### PRIORITAS TABEL (BY IMPORTANCE)

**CRITICAL (?? Wajib Ada):**

- documents ? Data dokumen (jantung sistem)
- rooms, cabinets, shelves, boxes, numbers, folders ? Hierarki lokasi (6 tabel)
- users ? User/login (autentikasi)

**IMPORTANT (?? Sangat Penting):**

- document_types ? Klasifikasi dokumen
- document_categories ? Kategori dokumen
- roles, permissions, model_has_roles ? RBAC

**SUPPORT (?? Pendukung):**

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
13. system_settings (independent)
14. audit_logs (FK: users)

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
- status (AVAILABLE/ARCHIVED/DAMAGED/DELETED)
- retention_year (berapa tahun disimpan)
- is_important (flag dokumen penting)

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
13. 2024_01_01_000013_create_system_settings_table
14. 2024_01_01_000014_create_audit_logs_table
15. 2024_01_01_000015_create_model_has_roles_table
16. 2024_01_01_000016_create_model_has_permissions_table

---

### RELASI ANTAR TABEL OVERVIEW

\\\
User (1) --[N]-? AuditLog
----[N]--? created_by (Rooms, Cabinets, etc)

DocumentType (1) --[N]-? Document
DocumentCategory (1) --[N]-? Document

Room (1) --[N]-? Cabinet --[N]-? Shelf --[N]-? Box --[N]-? Number --[N]-? Folder --[N]-? Document
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

### Konfigurasi & Sistem

**system_settings**

- id
- setting_key
- setting_value
- setting_type
- description
- status
- created_at
- updated_at
- deleted_at

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
   - `documents`, `system_settings`, `audit_logs`

### 27.2. Relasi Utama

- `rooms` 1 → N `cabinets`
- `cabinets` 1 → N `shelves`
- `shelves` 1 → N `boxes`
- `boxes` 1 → N `numbers`
- `numbers` 1 → N `folders`
- `folders` 1 → N `documents`
- `document_types` 1 → N `documents`
- `document_categories` 1 → N `documents`
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

**audit_logs**

- `user_id` ? FK `users.id`
- mencatat perubahan pada tabel penting dan operasi CRUD

### 27.4. Diagram Relasi Sederhana

```
rooms -> cabinets -> shelves -> boxes -> numbers -> folders -> documents
document_types -> documents
document_categories -> documents
users -> audit_logs
```

### 27.5. Keterangan Relasi

- Lokasi fisik menggunakan pohon 1 ke banyak dari `rooms` sampai `folders`.
- Setiap `document` disimpan di satu `folder`.
- `document_types` dan `document_categories` adalah master klasifikasi dokumen.
- `audit_logs` mencatat perubahan data dan operasi CRUD.

### 27.6. Contoh FK dan Index yang Direkomendasikan

- `cabinets.room_id` INDEX + FK -> `rooms.id`
- `shelves.cabinet_id` INDEX + FK -> `cabinets.id`
- `boxes.shelf_id` INDEX + FK -> `shelves.id`
- `numbers.box_id` INDEX + FK -> `boxes.id`
- `folders.number_id` INDEX + FK -> `numbers.id`
- `documents.folder_id` INDEX + FK -> `folders.id`
- `documents.document_type_id` INDEX + FK -> `document_types.id`
- `documents.document_category_id` INDEX + FK -> `document_categories.id`
- `audit_logs.user_id` INDEX + FK -> `users.id`

### 27.7. Catatan Implementasi

- Gunakan `soft deletes` pada tabel master dan dokumen untuk audit dan pemulihan.
- Gunakan `ENUM` atau `VARCHAR` untuk nilai status di `documents`.
- Gunakan `ON DELETE RESTRICT` atau `ON DELETE SET NULL` agar relasi lokasi tetap terjaga.

---

Credit: Muhamad Kosasih 2026

## 28. Implementasi dengan Laravel Livewire

### 28.1. Konsep Utama

Sistem ini dapat dibangun menggunakan Laravel Livewire untuk interface interaktif tanpa JavaScript framework kompleks. Livewire cocok untuk:

- CRUD data master lokasi (ruangan / lemari / rak / box / nomor / folder)
- CRUD dokumen dan upload file
- Form edit dan hapus dokumen
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
- `Document/DocumentDeleteConfirm`
- `Report/DocumentReport`

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
- `/admin/cabinets`
- `/admin/shelves`
- `/admin/boxes`
- `/admin/numbers`
- `/admin/folders`
- `/admin/document-types`
- `/admin/document-categories`
- `/admin/documents`
- `/admin/documents/create`
- `/admin/documents/{id}/edit`
- `/admin/documents/{id}/delete`
- `/admin/reports/documents`
- `/admin/reports/audit-logs`

Setiap route kemudian memanggil View Blade yang berisi komponen Livewire sesuai modul, misalnya `@livewire('location.room-index')`, `@livewire('document.document-index')`, atau `@livewire('report.document-report')`.

Contoh pengelompokan route:

1. **Location**

   - `/admin/rooms`
   - `/admin/cabinets`
   - `/admin/shelves`
   - `/admin/boxes`
   - `/admin/numbers`
   - `/admin/folders`

2. **Master Data Dokumen**

   - `/admin/document-types`
   - `/admin/document-categories`

3. **Dokumen**

   - `/admin/documents`
   - `/admin/documents/create`
   - `/admin/documents/{id}/edit`
   - `/admin/documents/{id}/delete`

4. **Laporan**
   - `/admin/reports/documents`
   - `/admin/reports/audit-logs`

Jika ingin lebih rapi, route juga bisa diberi nama:

- `admin.rooms.index`
- `admin.cabinets.index`
- `admin.shelves.index`
- `admin.boxes.index`
- `admin.numbers.index`
- `admin.folders.index`
- `admin.document-types.index`
- `admin.document-categories.index`
- `admin.documents.index`
- `admin.documents.create`
- `admin.documents.edit`
- `admin.documents.delete`
- `admin.reports.documents`
- `admin.reports.audit-logs`

### 28.8. Integrasi dengan Role & Permission

Untuk kontrol akses:

- `@can('view documents')` pada blade
- `if (! auth()->user()->can('edit documents')) abort(403);` di komponen Livewire
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

### 28.11. Fungsi CRUD per Modul

Berikut fungsi utama yang disarankan untuk masing-masing CRUD agar struktur komponen rapi dan konsisten.

#### A. CRUD Ruangan

Komponen: `Location/RoomIndex`

- `mount()` untuk inisialisasi data awal.
  - mengambil data ruangan dari database
  - mengisi variabel form dan filter default
  - menyiapkan mode tambah atau edit
- `render()` untuk menampilkan daftar ruangan.
  - mengirim data ruangan ke view
  - menerapkan pencarian dan pagination
- `resetForm()` untuk mengosongkan form input.
  - menghapus isi input kode, nama, dan deskripsi
  - mengembalikan status form ke mode normal
- `create()` untuk membuka mode tambah data.
  - mengaktifkan form tambah ruangan
  - menutup mode edit jika sedang aktif
- `store()` untuk menyimpan ruangan baru.
  - validasi kode dan nama ruangan
  - simpan data ke tabel `rooms`
  - catat `created_by` dan status aktif
  - tampilkan notifikasi sukses
- `edit($id)` untuk memuat data ruangan ke form.
  - mengambil data ruangan berdasarkan ID
  - mengisi form dengan data lama
  - mengubah mode form menjadi edit
- `update()` untuk menyimpan perubahan ruangan.
  - validasi data yang diubah
  - update record ruangan pada database
  - simpan `updated_by` atau log audit
- `confirmDelete($id)` untuk membuka dialog hapus.
  - menyimpan ID ruangan yang akan dihapus
  - menampilkan modal konfirmasi
- `destroy()` untuk menghapus ruangan.
  - cek apakah ruangan masih memiliki relasi lemari
  - jika aman, hapus secara soft delete
  - catat aktivitas pada audit log
- `search()` untuk mencari ruangan berdasarkan kode atau nama.
  - membaca input kata kunci
  - memfilter daftar ruangan secara real-time

#### B. CRUD Lemari

Komponen: `Location/CabinetIndex`

- `mount()` untuk memuat daftar ruangan sebagai relasi.
  - ambil data ruangan untuk dropdown
  - ambil data lemari awal untuk daftar
- `render()` untuk menampilkan daftar lemari.
  - menampilkan lemari beserta nama ruangan induk
  - menerapkan pencarian dan pagination
- `resetForm()` untuk reset state form.
  - kosongkan field input lemari
  - reset pilihan ruangan
- `create()` untuk tambah lemari baru.
  - buka form tambah lemari
  - set mode tambah aktif
- `store()` untuk menyimpan lemari.
  - validasi relasi ruangan dan kode lemari
  - simpan data lemari ke tabel `cabinets`
  - catat data pembuat
- `edit($id)` untuk edit lemari.
  - load data lemari ke form
  - siapkan form untuk update
- `update()` untuk memperbarui lemari.
  - validasi perubahan
  - update nama, kode, dan relasi ruangan
  - simpan log perubahan
- `confirmDelete($id)` untuk konfirmasi hapus lemari.
  - simpan ID lemari yang dipilih
  - tampilkan modal konfirmasi hapus
- `destroy()` untuk menghapus lemari.
  - cek apakah lemari masih dipakai oleh rak
  - soft delete jika aman
  - update audit log
- `loadRooms()` untuk mengambil data ruangan saat form dibuka.
  - mengambil daftar ruangan dari database
  - digunakan untuk dropdown relasi

#### C. CRUD Rak

Komponen: `Location/ShelfIndex`

- `mount()` untuk menyiapkan data lemari.
  - load daftar ruangan dan lemari
  - set default filter lokasi
- `render()` untuk menampilkan data rak.
  - menampilkan daftar rak beserta lemari induk
  - memproses pagination dan search
- `create()` untuk membuka form rak.
  - mengaktifkan mode tambah rak
- `store()` untuk menyimpan rak baru.
  - validasi lemari induk dan kode rak
  - simpan ke tabel `shelves`
- `edit($id)` untuk memuat data rak.
  - memuat data rak ke form edit
- `update()` untuk update rak.
  - update kode, nama, dan deskripsi rak
  - catat perubahan pada log
- `confirmDelete($id)` untuk konfirmasi hapus.
  - simpan ID rak untuk proses hapus
- `destroy()` untuk menghapus rak.
  - cek relasi box sebelum hapus
  - lakukan soft delete
- `loadCabinets($roomId)` untuk menampilkan lemari sesuai ruangan.
  - filter lemari berdasarkan ruangan terpilih
  - digunakan pada nested select

#### D. CRUD Box

Komponen: `Location/BoxIndex`

- `mount()` untuk memuat data rak.
  - load daftar rak dan lemari
- `render()` untuk menampilkan daftar box.
  - menampilkan box beserta nama rak induk
  - dukung pencarian
- `create()` untuk membuka form box.
  - membuka form tambah box
- `store()` untuk simpan box.
  - validasi relasi rak dan kode box
  - simpan ke tabel `boxes`
- `edit($id)` untuk edit box.
  - load data box ke form
- `update()` untuk update box.
  - perbarui data box yang dipilih
- `confirmDelete($id)` untuk hapus box.
  - tampilkan modal konfirmasi hapus box
- `destroy()` untuk menghapus box.
  - cek relasi nomor sebelum hapus
  - soft delete bila aman
- `loadShelves($cabinetId)` untuk ambil rak berdasarkan lemari.
  - filter rak sesuai lemari aktif

#### E. CRUD Nomor

Komponen: `Location/NumberIndex`

- `mount()` untuk menyiapkan data box.
  - load data box untuk dropdown
- `render()` untuk menampilkan nomor.
  - menampilkan daftar nomor per box
- `create()` untuk input nomor baru.
  - buka form tambah nomor
- `store()` untuk menyimpan nomor.
  - validasi kode nomor dan relasi box
  - simpan ke tabel `numbers`
- `edit($id)` untuk edit nomor.
  - memuat nomor ke form edit
- `update()` untuk update nomor.
  - update kode dan nama nomor
- `confirmDelete($id)` untuk hapus nomor.
  - menampilkan dialog konfirmasi
- `destroy()` untuk menghapus nomor.
  - cek apakah nomor masih memiliki folder
- `loadBoxes($shelfId)` untuk memuat box sesuai rak.
  - mengambil box berdasarkan rak yang dipilih

#### F. CRUD Map/Folder

Komponen: `Location/FolderIndex`

- `mount()` untuk inisialisasi data nomor.
  - load daftar nomor sebagai parent
- `render()` untuk menampilkan folder.
  - menampilkan daftar folder beserta nomor induk
- `create()` untuk membuka form folder.
  - aktifkan mode tambah folder
- `store()` untuk menyimpan folder baru.
  - validasi relasi nomor dan kode folder
  - simpan ke tabel `folders`
- `edit($id)` untuk edit folder.
  - load data folder ke form
- `update()` untuk memperbarui folder.
  - update kode, nama, dan deskripsi folder
- `confirmDelete($id)` untuk konfirmasi hapus.
  - simpan ID folder yang akan dihapus
- `destroy()` untuk menghapus folder.
  - cek relasi dokumen sebelum hapus
  - lakukan soft delete
- `loadNumbers($boxId)` untuk mengambil nomor berdasarkan box.
  - filter nomor berdasarkan box terpilih

#### G. CRUD Dokumen

Komponen: `Document/DocumentIndex` dan `Document/DocumentForm`

`DocumentIndex`:

- `mount()` untuk load data awal dan filter.
  - mengambil data dokumen, jenis, kategori, dan lokasi
  - menyiapkan keyword pencarian dan filter aktif
- `render()` untuk menampilkan daftar dokumen.
  - mengirim hasil pencarian ke view
  - menampilkan pagination dan tabel dokumen
- `search()` untuk pencarian berdasarkan nomor, nama, jenis, kategori, atau lokasi.
  - membaca keyword dan filter lokasi
  - mempersempit hasil query dokumen
- `resetFilter()` untuk mengosongkan filter pencarian.
  - menghapus keyword dan semua filter
- `openCreate()` untuk membuka form tambah dokumen.
  - membuka modal atau halaman tambah dokumen
- `openEdit($id)` untuk membuka form edit dokumen.
  - memuat data dokumen ke form edit
- `confirmDelete($id)` untuk membuka modal hapus.
  - menyimpan ID dokumen untuk konfirmasi hapus
- `deleteDocument()` untuk menghapus dokumen.
  - melakukan soft delete pada dokumen
  - mencatat audit log penghapusan

`DocumentForm`:

- `mount()` untuk isi data default saat tambah atau edit.
  - menyiapkan data jenis, kategori, dan lokasi
  - mengisi form saat mode edit
- `render()` untuk menampilkan form dokumen.
  - menampilkan field input dan dropdown bertingkat
- `loadLocations()` untuk memuat pilihan lokasi bertingkat.
  - memuat seluruh opsi lokasi dari ruangan sampai folder
- `updatedRoomId()` untuk memuat lemari berdasarkan ruangan.
  - reset pilihan turunan setelah ruangan berubah
  - ambil data lemari baru berdasarkan ruangan
- `updatedCabinetId()` untuk memuat rak berdasarkan lemari.
  - reset pilihan rak, box, nomor, dan folder
  - load data rak sesuai lemari
- `updatedShelfId()` untuk memuat box berdasarkan rak.
  - reset pilihan box, nomor, dan folder
  - load box sesuai rak aktif
- `updatedBoxId()` untuk memuat nomor berdasarkan box.
  - reset pilihan nomor dan folder
  - load nomor sesuai box aktif
- `updatedNumberId()` untuk memuat folder berdasarkan nomor.
  - reset pilihan folder
  - load folder sesuai nomor aktif
- `resetForm()` untuk mengosongkan field.
  - kosongkan seluruh input form
  - reset file upload dan error state
- `save()` untuk menyimpan dokumen baru.
  - validasi semua field dokumen
  - simpan file jika ada
  - insert data ke tabel `documents`
  - set status dokumen aktif
- `update()` untuk memperbarui data dokumen.
  - validasi perubahan data
  - update metadata, lokasi, atau file digital
  - simpan audit perubahan
- `uploadFile()` untuk menangani file digital.
  - validasi tipe dan ukuran file
  - menyimpan file ke storage yang sesuai

#### H. CRUD Hapus Dokumen

Komponen: `Document/DocumentDeleteConfirm`

- `mount($documentId)` untuk memuat data dokumen yang akan dihapus.
  - mengambil detail dokumen berdasarkan ID
  - menampilkan ringkasan sebelum hapus
- `render()` untuk menampilkan dialog konfirmasi.
  - menampilkan nama dokumen dan peringatan hapus
- `confirmDelete()` untuk menjalankan proses hapus.
  - mengubah status dokumen menjadi `DELETED` atau soft delete
  - mencatat waktu dan user yang menghapus
- `cancel()` untuk membatalkan penghapusan.
  - menutup dialog dan mengembalikan state awal

#### I. Laporan Dokumen

Komponen: `Report/DocumentReport`

- `mount()` untuk load filter awal.
  - menyiapkan filter tanggal, jenis, kategori, dan lokasi
- `render()` untuk menampilkan laporan.
  - menampilkan rekap dokumen dalam tabel atau kartu statistik
- `applyFilter()` untuk menerapkan filter laporan.
  - memproses filter yang dipilih user
  - menyusun query laporan
- `resetFilter()` untuk menghapus filter.
  - mengembalikan filter ke kondisi default
- `exportPdf()` untuk export laporan ke PDF.
  - menghasilkan file PDF dari data laporan aktif
- `exportExcel()` untuk export laporan ke Excel.
  - menghasilkan file Excel dari data laporan aktif

### 28.12. Pola Fungsi yang Disarankan

Setiap komponen CRUD sebaiknya mengikuti pola berikut:

1. `mount()` untuk load data awal.
2. `render()` untuk menampilkan view.
3. `create()` atau `openCreate()` untuk mode tambah.
4. `edit($id)` atau `openEdit($id)` untuk mode edit.
5. `store()` atau `save()` untuk simpan data baru.
6. `update()` untuk update data lama.
7. `confirmDelete($id)` untuk konfirmasi hapus.
8. `destroy()` atau `deleteDocument()` untuk hapus data.
9. `resetForm()` untuk membersihkan state komponen.

Pola ini membuat setiap CRUD lebih seragam, mudah dipelihara, dan lebih mudah dikembangkan ke tahap implementasi kode.
